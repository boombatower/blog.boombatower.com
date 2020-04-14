---
created: 1241138570
layout: post
redirect_from:
- article/58/
- node/58/
- automated-testing-system-2.0-new-features-part-1/
title: Automated Testing System 2.0 - New Features - Part 1
---
Over the next few weeks I plan to make a number of posts about the new features provided by ATS 2.0 and the benefits to the community. Currently, the system is in the <a href="/automated-testing-system-2.0-final-steps">final stages</a> of deployment, but is not yet active. Please be aware that these features will be available once ATS 2.0 has been deployed. I appreciate donations to the <a href="http://boombatower.chipin.com/mypages/view/id/d6208430185ee1a5">chipin</a> (right), as this project requires a lot of development time.

<b>Server management</b>

One of the major restraints holding back the expansion of the system has been the need to manually oversee the array of testing servers. The new system contains a number of enhancements to make it not only easier to manage the network, but also automates the task of adding new clients.

[![Client enable process](/files/pifr_client_enable_proccess.png)](/files/pifr_client_enable_proccess.png)

The most important addition that makes all this possible is the automatic client testing. Clients are automatically tested to ensure they are functioning properly. This is done through a set of tests that are sent to each test client with an expected result. The results the client sends back and compared with the expected result and that information is used to determine if the client is functioning properly. Clients are tested on a regular basis to ensure that they continue functioning as expected.

Another helpful change has been re-working the underlying architecture to use a <i>pull</i> based protocol instead of a <i>push</i> based protocol. This alleviates the issues caused when a client is unreachable for a period of time, or is removed without notice.


<b>Public server queue</b>

[![Add client](/files/pifr_client_add.png)](/files/pifr_client_add.png)

Another improvement that will facility a larger testing network is the public server queue. Allowing anyone to add a server to the network is possible since the clients are automatically tested as described above.

The interface has been designed so that users may control the set of machines that they have added to the network. The system automatically assigns the client a key that must be stored on the client and is used for authentication. The process of adding a client to the master list is very simple and should provide an easy way for users to donate servers.

If the system detects any issues with the client down the line, such as becoming out of date, it will notify the server administrator of the problem and disable the test client. The system will continually re-test the client and re-enable it automatically if it passes inspection. Alternatively, the server administrator may request the client to be tests immediately after fixing the issue.

<b>Multiple database support</b>

The new system has been abstracted to allow for the support of PostgreSQL and SQLite in addition to MySQL. This is vital to ensure that the Drupal 7 properly supports all three databases. Just as patches are not committed until they pass all the tests, patches will not be committed until after passing all the test on all three databases (5 environments with the database variations).

<style type="text/css">
.node div.image {
  float: right;
  margin: 7px;
  font-size: 85%;
  line-height: 1.5;
  width: 320px;
}

.node div.image img {
  width: 320px;
}
</style>
