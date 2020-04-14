---
created: 1241587161
layout: post
redirect_from:
- article/59/
- node/59/
- automated-testing-system-statistics/
title: Automated Testing System - Statistics
---
I decided to pull gather some statistics about the automated testing system. These statistics were collected on Wednesday, May 6, 2009 at 4:00 AM  GMT. Automatic generation of these statistics along with analysis is a feature I have in mind for ATS 2.0. I appreciate donations to the <a href="http://boombatower.chipin.com/mypages/view/id/d6208430185ee1a5">chipin</a> (right), as this project requires a lot of development time.

From the data you can see that the test slaves have been running tests for the equivalent of 200 days. The system has been running for 192 days and not all the data was included since some of it is inaccurate. That means the system has saved 200 days of developer's time! It is clear that the ATS is a vital part of test driven development. Additionally the time that would have been spent fixing regressions and new bugs has been drastically lowered.

<table>
<tr>
  <th>Item</th>
  <th>Function</th>
  <th>Value</th>
</tr>
<tr>
  <td>Time testing</td>
  <td>SUM</td>
  <td>17,310,047 seconds (~288,500 minutes, ~200 days)</td>
</tr>
<tr>
  <td>Test run (test suite)</td>
  <td>COUNT</td>
  <td>42,351</td>
</tr>
<tr>
  <td />
  <td>MAX</td>
  <td>3620 seconds (~60 minutes)</td>
</tr>
<tr>
  <td />
  <td>MIN</td>
  <td>17 seconds</td>
</tr>
<tr>
  <td />
  <td>AVG</td>
  <td>804 seconds (~ 13 minutes)</td>
</tr>
<tr>
  <td />
  <td>STDDEV_POP</td>
  <td>783 seconds (~13 minutes)</td>
</tr>
<tr>
  <td>Test (patch, times tested)</td>
  <td>COUNT</td>
  <td>6,953</td>
</tr>
<tr>
  <td />
  <td>MAX</td>
  <td>86</td>
</tr>
<tr>
  <td />
  <td>MIN</td>
  <td>1</td>
</tr>
<tr>
  <td />
  <td>AVG</td>
  <td>10</td>
</tr>
<tr>
  <td />
  <td>STDDEV_POP</td>
  <td>15</td>
</tr>
<tr>
  <td>Test pass count</td>
  <td>MAX</td>
  <td>11,453</td>
</tr>
<tr>
  <td />
  <td>MIN</td>
  <td>0</td>
</tr>
<tr>
  <td />
  <td>AVG</td>
  <td>4,265</td>
</tr>
<tr>
  <td />
  <td>STDDEV_POP</td>
  <td>4,910</td>
</tr>
<tr>
  <td>Test fail count</td>
  <td>MAX</td>
  <td>6,989</td>
</tr>
<tr>
  <td />
  <td>MIN</td>
  <td>0</td>
</tr>
<tr>
  <td />
  <td>AVG</td>
  <td>9</td>
</tr>
<tr>
  <td />
  <td>STDDEV_POP</td>
  <td>155</td>
</tr>
<tr>
  <td>Test exception count</td>
  <td>MAX</td>
  <td>813,795</td>
</tr>
<tr>
  <td />
  <td>MIN</td>
  <td>0</td>
</tr>
<tr>
  <td />
  <td>AVG</td>
  <td>160</td>
</tr>
<tr>
  <td />
  <td>STDDEV_POP</td>
  <td>9,893</td>
</tr>
</table>

One item you may notice is the maximum test exception count of 813,795. The <a href="http://testing.drupal.org/pifr/file/1/theme.inc_.head__0_1.patch">patch</a> that caused that many exceptions proved that our system is scalable! The patch is much appreciated. :)

Saved the current <a href="http://testing.drupal.org/pifr/stats">test result breakdown</a>.

<div style="text-align: center">
<img src="/files/result_distribution.png" alt="Result distribution" />
</div>

The average test run length for all the active test slaves can be seen below. This data is only looking at the latest test run for each patch in the system.

<table>
<tr>
  <th>Test slave</th>
  <th>Average test length*</th>
</tr>
<tr>
  <td>4</td>
  <td>730 seconds</td>
</tr>
<tr>
  <td>5</td>
  <td>1,753 seconds</td>
</tr>
<tr>
  <td>7</td>
  <td>1,352 seconds</td>
</tr>
<tr>
  <td>8</td>
  <td>576 seconds</td>
</tr>
<tr>
  <td>9</td>
  <td>2,438 seconds</td>
</tr>
<tr>
  <td>10</td>
  <td>1,942 seconds</td>
</tr>
<tr>
  <td>12</td>
  <td>1,161 seconds</td>
</tr>
<tr>
  <td>16</td>
  <td>217 seconds</td>
</tr>
</table>

\* Excludes test runs that do not pass initial checks and fail before running test suite.

<div style="text-align: center">
<img src="/files/average_test_length.png" alt="Average Test Length" />
</div>
