---
created: 1315552934
layout: post
redirect_from:
- article/84/
- node/84/
- handling-multicolumn-data-and-aggregated-using-drupal-7-fields/
title: Handling multicolumn and aggregated data using Drupal 7 fields
---
The following technique and many of the tools were developed/improved by <a href="http://reviewdriven.com/">ReviewDriven</a>.

<h2>Problems</h2>

The <a href="http://api.drupal.org/api/drupal/modules--field--field.module/group/field/7">field API</a> in <a href="http://drupal.org">Drupal</a> 7 combined with <a href="http://drupal.org/project/views">Views</a> is very powerful combination. Yet there are certain data structures that are difficult or inefficient to work with using the two tools. One such structure is tables of data with multiple columns which can be stored using the fields API, but have a number of issues that arise.

<ul>
  <li>Field data is loaded every time the entity is loaded which can be catastrophic with large datasets.
  <ul>
    <li>For example, when editing a node with a large dataset attached each value has a corresponding field element generated which eats up memory and processing power quickly and can easily cause white screens.</li>
    <li>When viewing a node even if the fields are hidden the data is loaded and again is a tax on memory.</li>
  </ul>
  </li>
  <li>Relationship between columns is not exported to Views, since it has no way to know, which limits the way values may be displayed.</li>
  <li>Aggregating columns in Views cannot be done across entities.</li>
  <li>Views loads field data through the field API which is inefficient for large datasets, but ensures that display formatters and such are respected.</li>
</ul>

<h2>Solutions</h2>

Thankfully, there are a number of tools that can be utilized to solve each of these problems and in combination provide a very powerful way to handle tables of data and aggregation across entities.

<h3><a href="http://drupal.org/project/field_suppress">Field suppress</a></h3>

From the Field suppress project page.
<blockquote>
Suppress field data from being loaded during entity_load(). Since field data will not be loaded it will not be displayed nor editable through the interface. This can be handy if you are using an alternate means to display or edit data and/or if you have a large amount of data in fields which will cause the node edit (or similar) interface to use a huge amount of memory and take a very long time to build.
</blockquote>

Field suppress solves the first problem, but data will then need to be edited directly through code and manually displayed (ex. Views).

<h3><a href="http://drupal.org/project/field_group_views">Field group views</a></h3>

The next problem requires an interface/API for defining relationships between fields (or columns). Relationships can be defined using Field group views which is a plugin for <a href="http://drupal.org/project/field_group">Field group</a> which provides both a UI and exportables for defining groups of fields. A display group can be defined and the plugin set to Views which will then export the proper relationships to Views and generate a stub view which can then be customized. The view will automatically replace the fields when displaying the entity. A requirement of Field group views is Views field which actually solves the last two problems.

<h3><a href="http://drupal.org/project/views_field">Views field</a></h3>

Views field exposes the field tables and revision field tables to Views as base tables. Using the field base tables means that field data can be loaded directly instead of going through the field API. Loading data directly is much more efficient (especially for large datasets) and allows for aggregation across multiple entities, but losses the formatting capabilities of the fields API (could possibly add support for formatting to Views field). Formatting can also be added to the exposed field base table through hook_views_data_alter() in the following manor.
```php
/**
 * Implements hook_views_data_alter().
 */
function conduit_views_data_alter(&$data) {
  // May not always be '_value' depending on field type.
  $data['field_data_MY_FIELD']['MY_FIELD_value']['field']['handler'] = 'views_handler_HANDLER';
}
```

<em>Please note, there are two <a href="http://drupal.org/project/issues/views_field?categories=bug">bugs</a> that cause annoyances due to changes in the Views API, but do no prevent Views field from working. Feel free to submit patches.<em>

<h2>Example</h2>

I have utilized these tools in combination on a number of projects with quite satisfying results, but I will attempt to provide a few generic examples to provide a clearer picture of how these tools can be used.

<h3>Tallying summary results</h3>

If you have a collection of entities and you want to be able to group the overall field data and perform SQL operations like COUNT() or SUM() Views field makes it easy. The core poll module could be rewritten using this technique so we will use poll as an example (could be done many ways).

Say you have a node type for "Foo" poll entries that looks something like the following.

<u>poll_foo_entry</u>
<ul>
  <li>poll_foo_value: customizable field capable of storing the value for a poll, in this case lets go with a text field</li>
</ul>

Results can be calculated using a view with the base table poll_foo_value and aggregation enabled. The poll_foo_value column can be used as the group by column and additional columns can be added for determining the COUNT(). You could even then display the results using a chart plugin for views.

I used this technique to create http://survey.reviewdriven.com/results. The tallied results are display on the left.

<h3>Table of data</h3>

Another powerful usecase is storing and displaying a table of data. Lets use a simple example of storing temperature data over time on nodes (possibly a node per region). A possible node structure is as follows.

<u>temperature_history</u>
<ul>
  <li>title: region or some such</li>
  <li>date: mulivalue <a href="http://drupal.org/project/date">date</a> field</li>
  <li>temperature: mulivalue <a href="http://drupal.org/node/1188262">temperature</a> field</li>
</ul>

The date and temperature fields can then be related using Field group views by placing them in the same group and displayed using a view. The fields are related based on each fields delta. In other words the data is stored using the field API in the following manor.

```
date[0] = 2011-09-01
date[1] = 2011-09-02
date[2] = 2011-09-03
date[3] = 2011-09-04
date[4] = 2011-09-06

temperature[0] = 70
temperature[1] = 69
temperature[2] = 68
temperature[3] = 67
temperature[4] = 66
```

The number in brackets being the delta which allows the tables to be joined intelligently and produce a table like the following.

<table>
<tr>
  <th>Date</th>
  <th>Temperature</th>
</tr>
<tr>
  <td>2011-09-01</td>
  <td>70</td>
</tr>
<tr>
  <td>2011-09-02</td>
  <td>69</td>
</tr>
<tr>
  <td>2011-09-03</td>
  <td>68</td>
</tr>
<tr>
  <td>2011-09-04</td>
  <td>67</td>
</tr>
<tr>
  <td>2011-09-06</td>
  <td>66</td>
</tr>
</table>

<h2>Improvements</h2>

There are still some areas for improvement, but one only has so much time.

<h3><a href="http://drupal.org/project/pbs">Per-Bundle Storage</a></h3>

Storing related field data in a single table allows for the removal of overlapping columns and removes the need to join multiple tables. A field group could then be placed in a separate bundle or otherwise exposed to PBS and stored together in a single table. The database scheme would then be much more manageable and easier to query manually.

<em>Please keep in mind that PBS is not currently functional.</em>

<h3>Editing</h3>

Being able to edit the dataset using a view would also be a major plus. There a number of modules that provide functionality of this sort, but not in such a flexible manor. Something like <a href="http://drupal.org/project/editview">Editview</a> would complete Field group views functionality. Large datasets could then be paginated for editing to prevent overload.

<h2>Exciting usage</h2>

Given that Views is plugable virtually an data structure could be displayed using this technique. The advantage over writing a one off field is that the structure is then easy to extend and modify, and requires little to no code to create. Simply export the fields, and field group definitions.

Fields such as the <a href="http://drupal.org/project/name">Name field</a> could be turned into a collection of fields displayed using a view and editable (assuming that gets implemented) using a similar structure.

The possibilities opened up by this technique are quite exciting and I look forward to seeing what people come up with.
