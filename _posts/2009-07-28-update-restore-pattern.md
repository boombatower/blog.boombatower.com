---
created: 1248823835
layout: post
redirect_from:
- article/64/
- node/64/
- update-restore-pattern/
title: Update restore pattern
---
Recently I have been working on confirming that the <a href="http://drupal.org/node/528234">Project Issue File Review (PIFR)</a> and <a href="http://drupal.org/node/532678">Project Issue File Testing (PIFT)</a> update paths from 1.x to 2.x work properly. I was attempting to do so with a copy of the live data from <a href="http://testing.drupal.org">testing.drupal.org</a> and <a href="http://drupal.org">drupal.org</a>. Doing so would ensure that the somewhat delicate upgrade path functioned properly.

I ran into the issue of resetting the data after a failed update attempt. Doing so is not a quick and simple task, especially for the PIFR upgrade which requires renaming the old tables (I may move into udpate path itself) so the data can be converted and placed in the new schema. To solve the problem I developed the following script, or pattern, that can be used to reset for another update attempt. The first script is the one used for the PIFT update which is much simpler. The <em>pift_data.sql</em> contains the live data from drupal.org and similar with the PIFR update.

I placed the scripts in index.php, just as I do with all my scripts, after the `drupal_bootstrap(DRUPAL_BOOTSTRAP_FULL);` line. The scripts can then be ivoked by appending <em>?restore=true</em> to the URL to your site. Obviously I commented out the one I was not using.

I found that having these scripts helped ensure that I tested the update path extensively and as such they seem like a good thing to share.

<b>PIFT update reset script</b>
```php
if (isset($_GET['restore'])) {
  header('Content-Type: text/plain');
  ini_set('display_errors', 1);
  set_time_limit(0);

  echo "resetting...\n";

  db_query('TRUNCATE TABLE {watchdog}');

  db_query("UPDATE {system}
            SET schema_version = %d
            WHERE name = '%s'", 6000, 'pift');

  $tables = array(
    'pift_data',
    'pift_test',
    'pift_project',
  );
  $ret = array();
  foreach ($tables as $table) {
    if (db_table_exists($table)) {
      db_drop_table($ret, $table);
    }
  }

  echo "importing d.o dataset...\n";

  $parts = parse_url($db_url['default']);
  $command = "mysql -u{$parts['user']} -p{$parts['pass']} " . substr($parts['path'], 1);
  exec($command . ' < /home/boombatower/download/pift_data.sql');

  echo "finished...\n";

  print_r($ret);

  exit;
}
```

<b>PIFR update reset script</b>
```php
if (isset($_GET['restore'])) {
  header('Content-Type: text/plain');
  ini_set('display_errors', 1);
  set_time_limit(0);

  echo "resetting...\n";

  db_query('TRUNCATE TABLE {watchdog}');

  db_query("UPDATE {system}
            SET schema_version = %d
            WHERE name = '%s'", 6000, 'pifr');

  $tables = array(
    'pifr_branch',
    'pifr_client',
    'pifr_file',
    'pifr_log',
    'pifr_project',
    'pifr_result',
    'pifr_result_detail',
    'pifr_result_detail_assertion',
    'pifr_test',
    'pifr_file_old',
    'pifr_result_old',
    'pifr_server',
    'pifr_log_old',
  );
  $ret = array();
  foreach ($tables as $table) {
    if (db_table_exists($table)) {
      db_drop_table($ret, $table);
    }
  }

  echo "importing t.d.o dataset...\n";

  $parts = parse_url($db_url['default']);
  $command = "mysql -u{$parts['user']} -p{$parts['pass']} " . substr($parts['path'], 1);
  exec($command . ' < /home/boombatower/download/tdo.sql');

  echo "renaming...\n";

  db_rename_table($ret, 'pifr_file', 'pifr_file_old');
  db_rename_table($ret, 'pifr_result', 'pifr_result_old');
  db_rename_table($ret, 'pifr_log', 'pifr_log_old');

  echo "importing pifr_server schema...\n";

  exec($command . ' < /home/boombatower/download/pifr_server.sql');

  echo "finished...\n";

  print_r($ret);

  exit;
}
```
