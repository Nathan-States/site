---
title: 'Analyzing 20 Years of PCAOB Inspections and Enforcement (Kaggle Starter Notebook)'
excerpt: 'Looking over the agency who audits the auditors.'
author: 'Nathan States'
date: '2022-10-14'
slug: []
categories: []
tags: []
---

<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/echarts-en.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/ecStat.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r/dataTool.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/echarts4r-binding/echarts4r.js"></script>
<style type="text/css">
.figcaption {
  font-size: 0.8em;
}

.body {
  background-color: #fcfcfc;
}

.img {
  height: auto;
  max-width: 100%;
  background-color: #fcfcfc;
}

.html {
  height: auto;
  max-width: 100%;
}

.full-width {
  left: 50%;
  margin-left: auto;
  margin-right: auto;
  max-width: 70vw;
  position: relative;
  right: 50%;
  width: 70vw;
}

.most-width {
  left: 50%;
  margin-left: -40vw;
  margin-right: -40vw;
  max-width: 80vw;
  position: relative;
  right: 50%;
  width: 80vw;
}

.box-width{
  left: 50%;
  margin-left: -35vw;
  margin-right: -35vw;
  max-width: 70vw;
  position: relative;
  right: 50%;
  width: 70vw;
}
</style>

This is an explanatory notebook for [this](google.com) *Kaggle* dataset. *Kaggle* does not allow for nicer and interactive charts, which is why I’m hosting this notebook here instead. This post is meant to explain what the PCAOB is, how the data was gathered, and to analyze them based on their inspection and enforcement actions.

## Background

At the end of 2000, the energy corporation *Enron* would finish the year as the seventh largest company in the United States. Less than a year later, though, the company would file for bankruptcy, [causing](https://pcaobus.org/news-events/speeches/speech-detail/background-on-the-pcaob_465) tens of thousands to lose employment, and wiping out over \$2.1 billion in 401(k) contributions. A later investigation by the [Department of Justice](https://www.justice.gov/osg/brief/arthur-anderson-llp-v-united-states-opposition) (**DOJ**) found that Enron had been engaged in massive accounting fraud and were consistently lying about how profitable the company was. Then CEO Jeffrey Skilling would earn a 24 year prison sentence for his role in the scandal.

Auditors, in simple terms, are supposed to stop such things from happening. After the Great Depression, when it was discovered numerous companies had fraudulently over-inflated their profit margins, exacerbating the effects of the economic depression, all publicly traded companies were required to audit themselves upon the creation of the SEC in 1934. Auditors are expected to act impartially, ensure the revenue corporations report to investors is accurate, and to notify the appropriate authorities when fraud is detected. However, the audit industry remained self-regulated, and misconduct was rarely, if ever disciplined.

The auditors who worked at Enron were **Arthur Andersen**, then the fifth largest audit firm in the United States. In 2002, they were indicted by the DOJ for [shredding documents](https://www.latimes.com/archives/la-xpm-2002-may-14-fi-andersen14-story.html) related to Enron, though they were ultimately never charged due to a [technicality regarding jury instructions](https://www.law.cornell.edu/supct/html/04-368.ZO.html). One might wonder what would compel Andersen to (*allegedly*) commit a serious felony by destroying potential evidence for one of its clients.

<img src="images/arthur-andersen.png" alt="Arthur Andersen Logo" width="45%" align="right" style="padding-left: 1em"/>

Not only was Andersen being paid \$25 million by Enron to essentially not do their jobs, but they were [also receiving](https://news.bloomberglaw.com/us-law-week/enrons-collapse-20-years-later-lessons-not-learned) \$27 million in *non-auditing* fees, which almost entirely consisted of consulting services. It should be noted that auditors doubling as consultants is a somewhat [recent phenomenon](https://pcaobus.org/news-events/speeches/speech-detail/the-rise-of-advisory-services-in-audit-firms_544). During the late 1990s and early 2000s, the **Big Four** audit firms (those being *Deloitte*, *PricewaterhouseCoopers*, *KPMG*, and *Ernst & Young*) began expanding their consulting services substantially, which turned out to be much more profitable than only auditing. This transition saw a noticeable drop in overall audit quality, however. Data from the Government Accountability Office gathered from a [*Cornell Law* study](https://scholarship.law.cornell.edu/facpub/968/) found that the total number of financial restatements, or instances in which a company had to readjust their financial reports, increased from *0.5%* in 1997 to **3.9%** in 2001 among the then Big Five.

To put this in perspective, among the five largest audit firms, the number of financial restatements increased from just *5* companies in 1997 to **40** in 2001. Another study by the [University of Michigan Business Board](http://d1c25a6gwz7q5e.cloudfront.net/papers/1072.pdf) found that on average, the share price of a stock fell **25%** following the announcement of a financial restatement from 1997 - 2001, meaning that (unsurprisingly) nearly all financial restatements were the results of corporations *exaggerating* their revenue.

While Enron was the catalyst that showed self-regulation was problematic, the overall wave of accounting scandals (namely led by Worldcom, Waste Management, and Tyco) would destroy investor confidence in the market, providing motivation for the government to act. In 2002, Congress would pass the **Sarbanes-Oxley Act**, which received unanimous approval in the Senate, and set stricter guidelines and provisions for the audit industry.

This Act also created the Public Company Accounting Oversight Board (**PCAOB**), which every audit firm is required to register with. The PCAOB is responsible for monitoring audit firms, conducting investigations, and disciplining misconduct when it occurs. Essentially, their job is to ensure that auditors are doing their jobs properly, and to make sure nothing like Enron happens again.

<br>

<img src="images/pcaob.png" width="512" style="display: block; margin: auto;" />

<br>

However, if the audit industry could be undermined, could the PCAOB as well? Many government agencies - most recently the [EPA](https://www.brookings.edu/blog/up-front/2020/12/15/the-trump-administrations-major-environmental-deregulations/) - have been thoroughly weakened to serve certain interests. **Is the PCAOB protecting ordinary investors, or are they serving special interests?** This post will attempt to answer these question by looking at PCAOB inspection reports and enforcement actions over the last twenty years.

### Inspiration

This post was inspired by this [article](https://www.pogo.org/investigation/2019/09/how-an-agency-youve-never-heard-of-is-leaving-the-economy-at-risk) by the **Project on Government Oversight**. Check out their full series on the PCAOB [here](https://www.pogo.org/series-collections/your-financial-security-on-the-line).

### Gathering Data

If you go to [pcaobus.org](https://pcaobus.org/), you’ll find two separate databases: [*inspections*](https://pcaobus.org/oversight/inspections/firm-inspection-reports) and [e*nforcement actions*](https://pcaobus.org/oversight/enforcement/enforcement-actions?enforcementordertypes=Settled%20Disciplinary%20Order). Both databases provide reports in PDF format, so I manually gathered the relevant details from each file and stored them in two separate spreadsheets.

The PCAOB has performed over 3,500 inspection reports since its creation in 2002, which would take an extremely long time to gather manually. For the sake of time, only data from the largest auditing firms are presented here.

According to the enforcement database, the PCAOB has given out a total of 345 *enforcement actions* as of October 22, 2022, all of which are included in this analysis. It should be noted that the way sanctions have been given has changed over time. Generally, each sanction is provided with its own individual report, but other times, one report will contain multiple sanctions (an example can be found [here](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2020-019-gt.pdf?sfvrsn=67d2d9e4_2)). This means that the number of PCAOB enforcement actions listed on their site doesn’t correspond with the total number of sanctions they’ve given, which is why this spreadsheet contains **502** rows instead of *345*.

------------------------------------------------------------------------

One last thing to note; the Big Four are by far the largest auditing firms in the world, and represent some of the [richest privately owned companies](https://en.wikipedia.org/wiki/List_of_largest_private_non-governmental_companies_by_revenue) in the world. They audit nearly half of all publicly traded corporations on the NYSE, including nearly every single company on the S&P 500, meaning they are responsible for auditing a [significant portion of the economy](https://www.cfo.com/accounting-tax/auditing/2022/06/auditing-big-four-market-share-sec-registrants-accounting/). Their influence on overall audit quality is therefore very significant, which is why these four firms will be the prime focus of this analysis.

<br>

<div class="box-width">

![](images/deloitte.jpg)

</div>

<br>

With all that said, we’ll begin by looking at how well auditors have performed under the new regulations set by the PCAOB.

<center>

## **Inspections**

</center>

Here, only the Big Four are presented, but if you want to see some of the other firms, you can toggle them in the legend on the bottom of the chart.

------------------------------------------------------------------------

<div class="most-width">

<div id="htmlwidget-1" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"theme":"","tl":false,"draw":true,"renderer":"canvas","events":[{"data":{"type":"legendUnSelect","name":"Crowe"}},{"data":{"type":"legendUnSelect","name":"BDO"}},{"data":{"type":"legendUnSelect","name":"Grant Thornton"}}],"buttons":[],"opts":{"yAxis":[{"show":true,"name":"Number of Failed Audits"}],"xAxis":[{"data":["2004","2005","2006","2007","2008","2009","2010","2011","2012","2013","2014","2015","2016","2017","2018","2019","2020"],"type":"category","boundaryGap":true,"name":""}],"legend":{"data":["BDO","Crowe","Deloitte","Ernst & Young","Grant Thornton","KPMG","PricewaterhouseCoopers"],"show":true,"type":"scroll","bottom":0.5},"series":[{"data":[{"value":["2004"," 11"]},{"value":["2005"," 18"]},{"value":["2006"," 29"]},{"value":["2007"," 34"]},{"value":["2008"," 40"]},{"value":["2009"," 48"]},{"value":["2010"," 56"]},{"value":["2011"," 65"]},{"value":["2012"," 76"]},{"value":["2013"," 91"]},{"value":["2014","108"]},{"value":["2015","120"]},{"value":["2016","136"]},{"value":["2017","145"]},{"value":["2018","156"]},{"value":["2019","167"]},{"value":["2020","180"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"BDO","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"},{"data":[{"value":["2004","11"]},{"value":["2005","19"]},{"value":["2006","22"]},{"value":["2007","24"]},{"value":["2008","25"]},{"value":["2009","27"]},{"value":["2010","35"]},{"value":["2011","43"]},{"value":["2012","49"]},{"value":["2013","54"]},{"value":["2014","59"]},{"value":["2015","62"]},{"value":["2016","67"]},{"value":["2017","70"]},{"value":["2018","71"]},{"value":["2019","78"]},{"value":["2020","82"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Crowe","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"},{"data":[{"value":["2004","  8"]},{"value":["2005"," 25"]},{"value":["2006"," 33"]},{"value":["2007"," 42"]},{"value":["2008"," 49"]},{"value":["2009"," 64"]},{"value":["2010"," 90"]},{"value":["2011","112"]},{"value":["2012","125"]},{"value":["2013","140"]},{"value":["2014","151"]},{"value":["2015","164"]},{"value":["2016","177"]},{"value":["2017","188"]},{"value":["2018","194"]},{"value":["2019","200"]},{"value":["2020","202"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Deloitte","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"},{"data":[{"value":["2004","  8"]},{"value":["2005"," 18"]},{"value":["2006"," 26"]},{"value":["2007"," 30"]},{"value":["2008"," 38"]},{"value":["2009"," 43"]},{"value":["2010"," 56"]},{"value":["2011"," 76"]},{"value":["2012","101"]},{"value":["2013","129"]},{"value":["2014","149"]},{"value":["2015","165"]},{"value":["2016","180"]},{"value":["2017","197"]},{"value":["2018","211"]},{"value":["2019","222"]},{"value":["2020","230"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Ernst & Young","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"},{"data":[{"value":["2004"," 15"]},{"value":["2005"," 27"]},{"value":["2006"," 35"]},{"value":["2007"," 40"]},{"value":["2008"," 49"]},{"value":["2009"," 54"]},{"value":["2010"," 66"]},{"value":["2011"," 79"]},{"value":["2012","101"]},{"value":["2013","121"]},{"value":["2014","132"]},{"value":["2015","146"]},{"value":["2016","154"]},{"value":["2017","160"]},{"value":["2018","168"]},{"value":["2019","175"]},{"value":["2020","180"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Grant Thornton","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"},{"data":[{"value":["2004"," 19"]},{"value":["2005"," 30"]},{"value":["2006"," 37"]},{"value":["2007"," 47"]},{"value":["2008"," 56"]},{"value":["2009"," 64"]},{"value":["2010"," 76"]},{"value":["2011"," 88"]},{"value":["2012","105"]},{"value":["2013","128"]},{"value":["2014","156"]},{"value":["2015","176"]},{"value":["2016","201"]},{"value":["2017","227"]},{"value":["2018","246"]},{"value":["2019","263"]},{"value":["2020","277"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"KPMG","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"},{"data":[{"value":["2004"," 30"]},{"value":["2005"," 39"]},{"value":["2006"," 45"]},{"value":["2007"," 51"]},{"value":["2008"," 57"]},{"value":["2009"," 66"]},{"value":["2010"," 94"]},{"value":["2011","120"]},{"value":["2012","141"]},{"value":["2013","160"]},{"value":["2014","177"]},{"value":["2015","189"]},{"value":["2016","200"]},{"value":["2017","213"]},{"value":["2018","227"]},{"value":["2019","245"]},{"value":["2020","246"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"PricewaterhouseCoopers","type":"line","coordinateSystem":"cartesian2d","areaStyle":[],"stack":"grp"}],"title":[{"left":"center","textStyle":{"fontSize":27},"text":"Big Four Have Failed 955 Audits From 2004-2020"}],"color":["#007a69","#00b5e2","#005eb8","#ef4136","#4d4e56","#ff7200","#a05eb5"],"tooltip":{"trigger":"axis","backgroundColor":"rgba(40, 40, 40, 0.75)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#fcfcfc"}},"textStyle":{"fontFamily":"Raleway"}},"dispose":true},"evals":[],"jsHooks":[]}</script>

</div>

Here a *failed audit* is, [as according to the PCAOB](https://pcaobus.org/Inspections/Documents/Inspections-Report-Guide.pdf):

> Deficiencies that were of *such significance* that we believe the firm, at the time it issued its audit report(s), had **not obtained sufficient appropriate audit evidence to support its opinion(s)** on the issuer’s financial statements and/or ICFR.

A *failed audit* doesn’t mean that fraud was detected, nor that the audit opinion itself was wrong. Rather, it means that if there *was* fraud, or if there *were* accounting irregularities, they would of gone undetected due to how poorly the audit was carried out.

From 2004-2009, the Big Four had failed 200 inspections. Within the next three years, this amount nearly doubled. This increase is felt mostly evenly across all firms before it begins tapering off in 2017. Interestingly, BDO and Grant Thornton have only failed slightly fewer audits than the largest firms, even though they are [much smaller](https://big4accountingfirms.com/top-10-accounting-firms/) in size comparatively. It seems the number of PCAOB inspections isn’t very proportionate to firm size, at least among the ten largest domestic firms.

Through the first decade, KPMG had failed the least number of audits among the Big Four. As of 2020, they have now failed the most, 31 more than PwC, the next highest. KPMG is the worst performing firm in terms of failing audit inspections in PCAOB history.

Each of these failed inspections could result in a potential sanction for the firm/auditors involved.

In terms of **failure rate**, KPMG is also the worst performing among the Big Four. Other large firms, such as BDO, performed *even* worse.

------------------------------------------------------------------------

<div class="most-width">

<div id="htmlwidget-2" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-2">{"x":{"theme":"","tl":false,"draw":true,"renderer":"canvas","events":[{"data":{"type":"legendUnSelect","name":"Crowe"}},{"data":{"type":"legendUnSelect","name":"BDO"}},{"data":{"type":"legendUnSelect","name":"Grant Thornton"}},{"data":{"type":"legendUnSelect","name":"RSM"}}],"buttons":[],"opts":{"yAxis":[{"show":true,"axisLabel":{"formatter":"function(value, index) {\n        var fmt = new Intl.NumberFormat('en', {\"style\":\"percent\",\"minimumFractionDigits\":0,\"maximumFractionDigits\":0,\"currency\":\"USD\"});\n        return fmt.format(value);\n    }"}}],"xAxis":[{"data":["2009","2010","2011","2012","2013","2014","2015","2016","2017","2018","2019","2020"],"type":"category","boundaryGap":true}],"legend":{"data":["BDO","Crowe","Deloitte","Ernst & Young","Grant Thornton","KPMG","PricewaterhouseCoopers","RSM"],"show":true,"type":"scroll","bottom":0.5},"series":[{"data":[{"value":["2009","0.2424242"]},{"value":["2010","0.2580645"]},{"value":["2011","0.3913043"]},{"value":["2012","0.5500000"]},{"value":["2013","0.6521739"]},{"value":["2014","0.7727273"]},{"value":["2015","0.5217391"]},{"value":["2016","0.6666667"]},{"value":["2017","0.3913043"]},{"value":["2018","0.4782609"]},{"value":["2019","0.4230769"]},{"value":["2020","0.5416667"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"BDO","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2009","0.15384615"]},{"value":["2010","0.61538462"]},{"value":["2011","0.61538462"]},{"value":["2012","0.50000000"]},{"value":["2013","0.38461538"]},{"value":["2014","0.35714286"]},{"value":["2015","0.21428571"]},{"value":["2016","0.33333333"]},{"value":["2017","0.18750000"]},{"value":["2018","0.07142857"]},{"value":["2019","0.50000000"]},{"value":["2020","0.26666667"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Crowe","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2009","0.20547945"]},{"value":["2010","0.44827586"]},{"value":["2011","0.41509434"]},{"value":["2012","0.25000000"]},{"value":["2013","0.28301887"]},{"value":["2014","0.20754717"]},{"value":["2015","0.23636364"]},{"value":["2016","0.23636364"]},{"value":["2017","0.20000000"]},{"value":["2018","0.13043478"]},{"value":["2019","0.11538462"]},{"value":["2020","0.03921569"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Deloitte","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2009","0.0862069"]},{"value":["2010","0.2063492"]},{"value":["2011","0.3571429"]},{"value":["2012","0.4807692"]},{"value":["2013","0.4912281"]},{"value":["2014","0.3571429"]},{"value":["2015","0.2909091"]},{"value":["2016","0.2727273"]},{"value":["2017","0.3090909"]},{"value":["2018","0.2592593"]},{"value":["2019","0.1833333"]},{"value":["2020","0.1538462"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Ernst & Young","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2009","0.1282051"]},{"value":["2010","0.2926829"]},{"value":["2011","0.3714286"]},{"value":["2012","0.6470588"]},{"value":["2013","0.5555556"]},{"value":["2014","0.3235294"]},{"value":["2015","0.4117647"]},{"value":["2016","0.2352941"]},{"value":["2017","0.1764706"]},{"value":["2018","0.2500000"]},{"value":["2019","0.2258065"]},{"value":["2020","0.1724138"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"Grant Thornton","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2009","0.1333333"]},{"value":["2010","0.2222222"]},{"value":["2011","0.2264151"]},{"value":["2012","0.3400000"]},{"value":["2013","0.4600000"]},{"value":["2014","0.5384615"]},{"value":["2015","0.3846154"]},{"value":["2016","0.4901961"]},{"value":["2017","0.5000000"]},{"value":["2018","0.3653846"]},{"value":["2019","0.2931034"]},{"value":["2020","0.2641509"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"KPMG","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2009","0.11842105"]},{"value":["2010","0.37333333"]},{"value":["2011","0.41269841"]},{"value":["2012","0.38888889"]},{"value":["2013","0.32203390"]},{"value":["2014","0.29310345"]},{"value":["2015","0.21818182"]},{"value":["2016","0.19642857"]},{"value":["2017","0.23636364"]},{"value":["2018","0.25454545"]},{"value":["2019","0.30000000"]},{"value":["2020","0.01923077"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"PricewaterhouseCoopers","type":"line","coordinateSystem":"cartesian2d"},{"data":[{"value":["2015","0.3333333"]},{"value":["2016","0.4666667"]},{"value":["2017","0.7333333"]},{"value":["2018","0.2941176"]},{"value":["2019","0.2000000"]},{"value":["2020","0.4666667"]}],"yAxisIndex":0,"xAxisIndex":0,"name":"RSM","type":"line","coordinateSystem":"cartesian2d"}],"title":[{"top":-5,"left":"center","textStyle":{"fontSize":27},"text":"Big Four Have An Average Fail Rate of 28.1%"}],"color":["#007a69","#00b5e2","#005eb8","#ef4136","#4d4e56","#ff7200","#a05eb5"],"tooltip":{"trigger":"axis","backgroundColor":"rgba(40, 40, 40, 0.75)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#fcfcfc"},"formatter":"function(params, ticket, callback) {\n        var fmt = new Intl.NumberFormat('en', {\"style\":\"percent\",\"minimumFractionDigits\":0,\"maximumFractionDigits\":0,\"currency\":\"USD\"});\n        var res = params[0].value[0];\n        for (i = 0; i < params.length; i++) {\n            res += '<br />' +\n                   params[i].marker + ' ' +\n                   params[i].seriesName + ': ' +\n                   fmt.format(parseFloat(params[i].value[1]));\n        }\n        return res;\n    }","axisPointer":{"label":"function(params, ticket, callback) {\n        var fmt = new Intl.NumberFormat('en', {\"style\":\"percent\",\"minimumFractionDigits\":0,\"maximumFractionDigits\":0,\"currency\":\"USD\"});\n        var res = params[0].value[0];\n        for (i = 0; i < params.length; i++) {\n            res += '<br />' +\n                   params[i].marker + ' ' +\n                   params[i].seriesName + ': ' +\n                   fmt.format(parseFloat(params[i].value[1]));\n        }\n        return res;\n    }"}},"textStyle":{"fontFamily":"Raleway"}},"dispose":true},"evals":["opts.yAxis.0.axisLabel.formatter","opts.tooltip.formatter","opts.tooltip.axisPointer.label"],"jsHooks":[]}</script>

</div>

<br>

It should be noted the PCAOB oddly did not track the total number of audits they reviewed for each firm until 2009, only reporting the total number of failed audit inspections, so there’s no way to know the failure rates for 2004 - 2008.

<center>

#### Big Four \| **2009 - 2020**

</center>

| Firm          | 2022 Revenue   | Audits Inspected | Audits Failed | Average Fail Rate |
|---------------|----------------|------------------|---------------|-------------------|
| Deloitte      | \$59.3 billion | 656              | 153           | 23.323%           |
| PwC           | \$50.3 billion | 718              | 189           | 26.323%           |
| Ernst & Young | \$45.4 billion | 673              | 192           | 28.529%           |
| KPMG          | \$32.3 billion | 637              | 221           | 34.694%           |
| **TOTALS**    | —              | **2684**         | **755**       | **28.130%**       |

It may come as a surprise that even the best performing firms are failing nearly **one in every four** audit inspections. The PCAOB states that audit inspections are chosen based on “perceived risk” and are not a representative sample on overall audit performance, so these rates are almost certainly overestimates. Reports in the last three years now include audits that are chosen randomly for inspection, which firms tend to perform better on.

<center>

## **Enforcement**

</center>

<div class="box-width">

![](images/gavel.jpg)

</div>

According to the [Sarbanes Oxley Act](https://pcaobus.org/About/History/Documents/PDFs/Sarbanes_Oxley_Act_of_2002.pdf):

> If the Board finds, based on all of the facts and circumstances, that a registered public accounting firm or associated person thereof has engaged in any act or practice, or omitted to act, in violation of this Act \[…\] the Board may impose such disciplinary or remedial sanctions as it determines appropiate.

These sanctions include a firm/auditor being barred from the accounting industry, limiting of business activities, and mandatory education courses. It could also involve a monetary fine, equal to:

> 1)  not more than **\$100,000** for a *natural person* or **\$2,000,000** for *any other person*; and
> 2)  in any case to which paragraph (5) applies, not more than **\$750,000** for a *natural person* or **\$15,000,000** for *any other person*;

Paragraph 5 refers to cases of:

> 1)  **intentional or knowing conduct**, including *reckless conduct*, that results in violation of the applicable statutory, regulatory, or professional standard; *or*
> 2)  **repeated instances of negligent conduct**, each resulting in a violation of the applicable statutory, regulatory, or professional standard.

The PCAOB serves under the SEC, which means the Federal Civil Penalties Inflation Adjustment Act Improvements Act of 2015 also applies to the PCAOB. This [Act](https://www.sec.gov/rules/other/2022/33-11021.pdf) adjusts monetary fines for inflation, which means maximum penalties are actually about 40% higher than the amounts listed above.

### **Overview**

In over 20 years of operation, the PCAOB has issued a total of **\$29,877,000** in fines. This includes all domestic and international firms, including sanctions against their auditors. Had the PCAOB fined each firm \$2 million for each failed audit inspection, (which they have the authority to do) they could of fined the Big Four alone \$1.91 **b**illion, and even more when considering all firms.

However, the Big Four don’t even make up the majority of PCAOB fines - at least their domestic affiliates.

Even though the [vast majority](https://pcaobus.org/oversight/inspections/firm-inspection-reports?country=United%20States) of inspection reports are conducted on domestic firms, the United States make up just **42.51%** of all PCAOB fines.

------------------------------------------------------------------------

<div class="box-width">

<img src="charts/chart2.png" width="1800" />

</div>

It might be confusing why the PCAOB - an American regulator - are sanctioning audit firms in other countries. The PCAOB will often enter [cooperative arrangements](https://pcaobus.org/oversight/international/regulatorycooperation) with other countries to “*minimize administrative burdens and potential legal or other conflicts that non-U.S. firms may face in their home countries*.” If an international firm, often some affiliate of a larger domestic company, wants to audit a US company, or do related US work, it’s easier to go through the PCAOB than relying on domestic regulators, which can have conflicting rules depending on the country. Agreeing to these arrangements makes those firms subjected to potential sanctions.

PCAOB enforcement actions against international firms/auditors is a more recent phenomenon that didn’t truly take off until 2016, a year which was headlined by the *Deloitte Brazil* scandal.

#### **Deloitte Brazil**

Brazil has been fined more than any country besides the United States, and nearly all of it comes from a single \$8,000,000 fine imposed on them in December 2016. This represents the **largest monetary fine in PCAOB history**. It’s also the first and only time the PCAOB ever used the Paragraph 5 part of the Act to fine a firm a higher amount.

According to the PCAOB report, **Deloitte Touche Tohmatsu Auditores Independentes** (or *Deloitte Brazil*) [knowingly issued false audit reports](https://pcaobus.org/news-events/news-releases/news-release-detail/pcaob-announces-8-million-settlement-with-deloitte-brazil-for-violations-including-issuing-materially-false-audit-reports-and-12-individuals-also-sanctioned-for-various-violations_601) in 2010 regarding **Gol Linhas Aereas Inteligentes S.A.** (*GOL Intelligent Airlines*), a Brazilian airline whose stock is listed on both the NYSE and BM&F Bovespa exchange, the major Brazilian stock exchange. When the company announced a financial restatement in August 2011, the share price for GOL dropped 65%, and the PCAOB launched an investigation the very next year.

Instead of complying with the PCAOB, senior managers at *Deloitte Brazil* “attempted to cover up its violations by improperly altering documents in connection with a 2012 PCAOB inspection” and by “obstructing a subsequent investigation.” Twelve auditors were sanctioned, and eleven of them were permanently barred from the accounting industry. You can read the full report of the incident [here](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2016-031-deloitte-brazil.pdf?sfvrsn=96e25af2_0).

<br><br>

The *Deloitte Brazil* case greatly skews the differences in totals between domestic and international firms, though even if we ignore it, there isn’t much a difference in terms of totals. This is significant, because the *number* of sanctions against international firms is much lower.

------------------------------------------------------------------------

<div class="most-width">

<img src="charts/chart1.png" width="2400" />

</div>

<br>

Something I’d like to emphasize is the extreme lack of consistency in the number of sanctions given each year. Not only did the PCAOB wait nearly three years before issuing a sanction, which included a span of time in which **PwC failed thirty audit inspections in 2004**, they only issued six enforcement actions for the entire year. The PCAOB followed this up by issuing six sanctions again in 2006, which means they actually issued sanctions at a slower pace than the previous year. It should be noted that none of these sanctions involved a monetary fine.

2007 was the first year that saw the PCAOB deliver its first monetary fine; **\$1,000,000** against Deloitte for its improper conduct surrounding its audit of *Ligand Pharmaceuticals*. According to the PCAOB [report](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/12-10_deloitte.pdf?sfvrsn=93ab40f3_0), Deloitte:

> “\[…\] failed to exercise due professional care in the performance of the \[2003\] audit and failed to obtain sufficient competent providential matter to support the opinion expressed in the audit report.”

It had been known that one of the audit partners involved in the case had produced poor audits for the company in the past, but he was chosen for the 2003 audit regardless.

In [2005](https://www.nytimes.com/2007/12/11/business/11account.html), *Ligand* restated their 2003 financial earnings, declaring that more than half the revenue for 2003 should not have been reported. From April 2004 - April 2005, the share price of *Ligand Pharmaceuticals* fell more than **75%**, and it would take **thirteen years** for the share price to reach its 2004 high again.

After seven more years of tepid enforcement, the number of sanctions increased dramatically from 2015 - 2017, especially against international firms. What the cause of this increase isn’t clear, though the subsequent decline could be explained by a sudden change in leadership. At the end of 2017, a completely [new PCAOB board](https://www.complianceweek.com/sec-appoints-all-new-board-to-lead-pcaob-in-2018/8970.article) was elected, which likely explains the drop in sanctions that continues today.

------------------------------------------------------------------------

To quickly summarize; most enforcement actions have been against domestic firms/auditors, but sanctions against international firms/auditors have resulted in much higher monetary penalties on average. Sanctions against international firms/auditors have increased significantly after 2016. The number of sanctions given each year - and potentially the kind of sanction - appears to be dependent on who the board members are.

### **Sanctions Against the Big Four**

Here, we’ll analyze sanctions against the Big Four (*Deloitte*, *PricewaterhouseCoopers*, *KPMG*, and *Ernst & Young*), comparing them down between domestic and international firms/auditors. We will start with domestic firms.

<br><br>

<center>
<img src="tables/table1.png" width="598" />
</center>

In total, domestic Big Four firms have been fined a grand total of **\$6,500,000** across *five* different sanctions. This covers roughly 21.76% of total monetary fines given by the PCAOB. KPMG, who has performed the worst among the Big Four, including a stretch from 2014-2017 where they failed an average of **47.75%** audit inspections - has never received a sanction against one of its domestic firms. This can not be said of that of its international firms.

### International Affiliates

<center>
<img src="tables/table2.png" width="864" />
</center>

Even if we were to ignore the \$8 million Deloitte fine, international affiliates of the Big Four have been fined a total of *\$7,340,000* - still more than their domestic counterparts - for a grand total of **\$15,340,000**, which covers the majority of PCAOB fines at *51.34%*.

The number of sanctions is much higher at 25, which occur through a variety of different countries and incidents. Five sanctions were leveled against five different PwC affiliates in India, all resulting from a single incident. Three sanctions against Canadian firms; two of them being Deloitte, and another against PwC. No other country appears more than twice, so there’s quite a lot of diversity in this sense.

### Domestic Auditors

<center>
<img src="tables/table3.png" width="763" />
</center>

Monetary penalties against auditors of the Big Four total **\$530,000**. In 9 out of the 23 (or 39.13%) cases, no monetary penalty was given. Ernst & Young has received the plurality of sanctions, while the other three firms have received a similar number of sanctions and fine totals (3-5 sanctions with fines totaling \$100,000 - \$110,000).

Scott Marcello, who used to head the Vice Chair of Audit at KPMG, is the only domestic monetary fine handed out to KPMG in the history of the PCAOB. It’s also the largest sanction ever given to an individual auditors by the PCAOB, and the case is worth elaborating on.

#### KPMG Scandal

[On January 23, 2018](https://www.justice.gov/usao-sdny/pr/5-former-kpmg-executives-and-pcaob-employees-charged-manhattan-federal-court-fraudulent), five former KPMG executives were charged with conspiracy to defraud the United States by attempting to steal confidential information from a former PCAOB employee, and then use that information to fraudulently improve their inspection results. They were also charged multiple times with conspiracy to commit wire fraud, and some of them could have been sent to prison for up to 85 years.

<img src="images/kpmg.png" alt="KPMG Logo" width="55%" align="left" style="padding-left: 1em"/>

Brian Sweet, the previously mentioned PCAOB employee who agreed to comply with the government, accepted a position at KPMG in 2017, which more than [doubled his salary](https://www.pogo.org/investigation/2020/01/how-accountants-took-washingtons-revolving-door-to-a-criminal-extreme). Before Sweet left the PCAOB, he had stolen several documents, including an external hard drive, that contained information on audits the PCAOB were planning to inspect at KPMG. He would later testify in court that one of the executives at KPMG had approached him at his office and “leaned against the door and asked me to give him the list” - the list being the upcoming PCAOB inspections.

In the end, only three of the six involved in the scandal received jail time, the longest sentence being just one year and one day. KPMG was fined \$50 million, while Marcello was sanctioned and fined \$100,000 for failing to “*supervise associated persons of KPMG \[…\] who illegally obtained and used confidential PCAOB information in violation of PCAOB rules and provisions of the securities laws…*” It is interesting that despite KPMG’s remarkably high audit inspection failure rate, and the fact they’ve never received a sanction against one of its domestic firms, executives would still be willing to commit a felony to improve performance on inspection reports.

### International Auditors

<center>
<img src="tables/table4.png" width="799" />
</center>

Total monetary fines against international auditors of the Big Four are just slightly lower, totaling \$490,000. 44.74% of sanctions involved no monetary fine, though the total number of sanctions is higher. It should be noted that many of these cases were related to the aforementioned *Deloitte Brazil* case. Even discarding this case, a disproportionate number of sanctions are against Deloitte affiliates. Total monetary fines are slightly lower against international auditors.

<br><br>

So far, we’ve covered **\$22,860,000** in fines across *91* different sanctions. You may of noticed that the conclusions reached in the overview don’t represent the breakdown of sanctions against the Big Four. While we would of expected most enforcement actions to be against domestic firms, with monetary fines against international affiliates slightly lower, it seems *both* international Big Four firms & auditors make up the majority of PCAOB sanctions and fines.

What this essentially represents is a fundamental backwardness in terms of enforcement; smaller firms with less influence/oversight are sanctioned more often and more harshly than their much larger domestic counterparts. While the PCAOB rarely sanctions international firms outside the Big Four, their tendency to focus sanctions on smaller firms is seen throughout the remaining enforcement actions.

## **Everything Else**

There are a remaining *411* sanctions spread across **\$7,017,000** left in the data. Similar to before, the breakdown is skewed between a few large firms receiving most of the monetary penalties.

### Other Firms

![](charts/chart3.png)

Domestic firms outside the Big Four have been sanctioned *125* times for a total of **\$4,304,500**. As of 2022, in terms of revenue, Grant Thornton is 7th largest, BDO is 5th, and Marcum is 15th. Next after those three are *Sole Proprietors*, i.e., signle ran businesses, who have been fined a total of **\$563,000** across 70 different infractions. It’s yet another example of the PCAOB disciplining smaller firms at a much higher rate than larger ones, though monetary fines are lower on average.

International firms outside the Big Four have been sanctioned more times than international Big Four affiliates, (*35* companies compared to the latter 25) but they’ve been fined a much lower amount; just *\$984,500* compared to \$15,340,000. Most of this comes from a single \$500,000 fine against BDO, so even this amount is skewed.

### Other Auditors

The absolute majority of PCAOB enforcement actions have been against non-Big Four domestic auditors, who have been sanctioned **221** times. Including Sole Proprietors from before, a staggering *57.97%* of PCAOB sanctions are against small, domestic auditors.

------------------------------------------------------------------------

If we look at the [international oversight map](https://pcaobus.org/oversight/international) provided by the PCAOB, while the PCAOB has inspected over 6,200 audit opinions in the United States, the rest of the countries partnered have issued well less than a 1,000 total. The disparity between these two groups can only be reasonably explained if the international cases contained more egregious conduct - but this is difficult to ascertain for reasons I will explain later.

### Secrecy

The PCAOB is unusually secretive in terms of the information it provides to the public. While the SEC (who the PCAOB serves under) makes disciplinary conduct publicly available - even if sanctions are not pursued - the PCAOB does not. This means that initial charges, hearings, and appeals are kept confidential, even in cases where sanctions are pursued. This also carries over into inspection reports. While the *number* of audits failed is publicly provided, the *firms* whose audits were failed **aren’t** made available to the public, instead being listed as “Issuer A,” “Issuer B,” and so on.

In 2011, senator Jack Reed (RI-D) introduced the **[PCAOB Enforcement Transparency Act of 2011](https://www.congress.gov/bill/112th-congress/senate-bill/1907?s=1&r=61)**, which would make disciplinary proceedings publicly available. It was never voted on, but it continually gets reintroduced at the start of every new Congress, meaning there is a near identical bill introducted in 2011, 2013, 2015, 2017, 2019, and now 2021, when the bill received bipartisan support when four decade veteran Chuck Grassley (IA-R) cosponsored it.

The year after the Act was first introduced, the Big Four spent more money on lobbying than they’ve did in [history](https://www.reuters.com/article/us-usa-accounting-big4/insight-big-4-auditors-spend-more-than-ever-on-u-s-lobbying-idUSBRE82C0JQ20120313), aimed mostly at influencing the PCAOB, spending a combined total of \$9,400,000. To show show some examples of what lobbying looks like; [in 2017](https://www.promarket.org/2017/05/12/global-audit-firms-led-deloitte-using-lobbying-clout-dilute-sarbanes-oxley-reforms/), when the Act was reintroduced again, Deloitte gave \$560,000 to lobby legislators for the “modernization of independence requirements.” It’s not immediately clear what this means. KPMG gave \$80,000 to a financial service group to watch for “language attempting to require the \[PCAOB\] to hold its disciplinary proceedings in public, and any other provisions pertaining to the PCAOB or its oversight.”

In terms of lobbying, these amounts are much less than other industries, especially pharmaceutical companies, who spend hundreds of millions each election cycle. There is, though, clearly a desire to limit public information by the auditing industry, which in turn, limits the level of analysis one can perform.

To put it simply, while the disparity between the amount Big Four international firms/auditors have been fined compared to domestic ones seems imbalanced, the extent of that imbalance is hard to determine without more details.

------------------------------------------------------------------------

## Everything Else

<div class="box-width">

<img src="charts/chart3.png" width="1800" />

</div>

In total, non-Big Four firms have been fined a total of \$4,304,500 across 125 sanctions. The majority of this comes from Grant Thornton, who has been fined more than PricewaterhouseCoopers, KPMG, and Ernst & Young - despite failing the least number of audits between the four, having a lower fail rate than KPMG, and having a similar fail rate to Ernst & Young (though PwC has consistently performed better than Grant Thornton). Marcum has a significantly worse fail rate than all other firms, which could explain their relatively high fines, though they’ve only received less in monetary fines than KPMG.

The next firms who have received the most in fines are Whitley Penn and Citrin Cooperman, some of the next largest firms in the United States. Both totals are the result of individual incidents that also resulted in multiple auditors being barred from the industry; you can view those reports [here](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2020-002-whitley-penn.pdf?sfvrsn=c8c9828c_0) and [there](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2022-007-citrin.pdf?sfvrsn=caca3f8b_4), respectively.

However, the fourth largest “company” are **Sole Proprietors**, or businesses ran by a single person. They make up 70 of the 125 sanctions, the second largest group to receive the most sanctions.

### Sanctions Against Domestic Auduitors

The *largest* group who has received the most sanctions are **domestic auditors working outside the Big Four**. They’ve received 221 sanctions, or about 44.02% of all total sanctions. Including Sole Proprietors from before, we see the majority of PCAOB enforcement actions have been against individuals working at smaller firms, contrary to what one might expect.

Including domestic firm totals from before, *domestic* Big Four firms & auditors have been fined **\$7,030,000** while all other *domestic* firms & auditors have been fined **\$5,669,500**. So while the former has been fined slightly more, it’s not that big of a difference.

### Sanctions Against International Firms & Auditors

With that said, while Big Four international firms have received a significant amount in fines (relative to the total, anyways), all other international firms & auditors have been sanctioned 65 times for \$1,097,000.

Nearly half of that amount comes from a single instance in *[BDO Mexico](https://pcaobus.org/Enforcement/Decisions/Documents/105-2019-028-BDO-Mexico.pdf)*, which saw six auditors sanctioned, four of them fined a total of \$30,000, and the firm fined \$500,000. The next highest fine against a firm is \$50,000. Sanctions against international auditors total 30, which is actually more than Big Four domestic auditors, though their monetary total is lower at \$112,500.

Once again, it should be noted that these international totals are the results of a substantial fewer number of audit reviews/investigations.

## Conclusions

<div class="most-width">

<div id="htmlwidget-3" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-3">{"x":{"theme":"","tl":false,"draw":true,"renderer":"canvas","events":[],"buttons":[],"opts":{"legend":{"data":["Big Four (Domestic)","Big Four (International)","Other Firms (Domestic)","Other Firms (International)","Auditors, Big Four (Domestic)","Auditors, Big Four (International)","Auditors, Other (Domestic)","Auditors, Other (International)"],"show":true,"type":"plain","bottom":0},"series":[{"name":"Total","type":"pie","radius":["50%","70%"],"data":[{"value":6500000,"name":"Big Four (Domestic)"},{"value":15340000,"name":"Big Four (International)"},{"value":4304500,"name":"Other Firms (Domestic)"},{"value":984500,"name":"Other Firms (International)"},{"value":530000,"name":"Auditors, Big Four (Domestic)"},{"value":490000,"name":"Auditors, Big Four (International)"},{"value":1365500,"name":"Auditors, Other (Domestic)"},{"value":112500,"name":"Auditors, Other (International)"}]}],"title":[{"left":"center","textStyle":{"fontSize":27},"text":"Total Monetary Fines by Group"}],"color":["#007a69","#00b5e2","#005eb8","#ef4136","#4d4e56","#ff7200","#a05eb5","#f84ec1"],"tooltip":{"trigger":"item","backgroundColor":"rgba(40, 40, 40, 0.75)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#fcfcfc"},"formatter":"function(params, ticket, callback) {\n        var fmt = new Intl.NumberFormat('en', {\"style\":\"currency\",\"minimumFractionDigits\":0,\"maximumFractionDigits\":0,\"currency\":\"USD\"});\n        var value = parseFloat(params.value) || 0;\n        return params.marker + ' ' +\n               params.name + ': ' + fmt.format(value);\n    }"},"textStyle":{"fontFamily":"Raleway"}},"dispose":true},"evals":["opts.tooltip.formatter"],"jsHooks":[]}</script>

</div>

### Number of Sanctions by Group

<div class="most-width">

<div id="htmlwidget-4" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-4">{"x":{"theme":"","tl":false,"draw":true,"renderer":"canvas","events":[],"buttons":[],"opts":{"legend":{"data":["Big Four (Domestic)","Big Four (International)","Other Firms (Domestic)","Other Firms (International)","Auditors, Big Four (Domestic)","Auditors, Big Four (International)","Auditors, Other (Domestic)","Auditors, Other (International)"],"show":true,"type":"plain","bottom":0},"series":[{"name":"Total","type":"pie","radius":["50%","70%"],"data":[{"value":5,"name":"Big Four (Domestic)"},{"value":25,"name":"Big Four (International)"},{"value":125,"name":"Other Firms (Domestic)"},{"value":35,"name":"Other Firms (International)"},{"value":23,"name":"Auditors, Big Four (Domestic)"},{"value":38,"name":"Auditors, Big Four (International)"},{"value":221,"name":"Auditors, Other (Domestic)"},{"value":30,"name":"Auditors, Other (International)"}]}],"title":[{"left":"center","textStyle":{"fontSize":27},"text":"Number of Sanctions by Group"}],"color":["#007a69","#00b5e2","#005eb8","#ef4136","#4d4e56","#ff7200","#a05eb5","#f84ec1"],"tooltip":{"trigger":"item","backgroundColor":"rgba(40, 40, 40, 0.75)","borderColor":"rgba(0, 0, 0, 0)","textStyle":{"color":"#fcfcfc"}},"textStyle":{"fontFamily":"Raleway"}},"dispose":true},"evals":[],"jsHooks":[]}</script>

</div>

<br>

-   While the Big Four have received the majority of **monetary fines** at \$6,500,000, international affiliates have received the majority of them at **\$15,340,000**, though this is heavily skewed as the result of the \$8,000,000 *Deloitte Brazil* sanction (even though they would still maintain a majority without its inclusion).
-   These discrepancies exist despite the PCAOB conducting far *fewer* audit inspections of its international affiliates than domestic ones. This is true for all international firms.
-   Domestic firms outside the Big Four have been fined \$4,304,500, about \$2.2 million **less** than domestic Big Four firms. They *number* of sanction they’ve received is **much** higher, having received 125 compared to just 5 for the Big Four.
-   Domestic auditors outside the big four have been fined more than all Big Four auditors *combed* @ \$1,365,500, compared to \$920,000.
-   Domestic auditors have also received the most *number* of sanctions, comprising **44.02%** of total all PCAOB enforcement actions. An **additional** 70 sanctions were against Sole Proprietors.

### Limitations

International firms of the Big Four were **five times** more likely to be sanctioned, and have received more than *double* the amount in monetary fines. This is despite a much smaller sample size in terms of audit inspections.

Is the PCAOB more strict for its international affiliates? Does the PCAOB more time disciplining certain groups over others? It’s difficult to answer these questions due to the overall lack of information available. The PCAOB doesn’t reveal any details on infractions that don’t result in sanctions, and inspection reports are often brief. Because of this, it’s possible that the cases that resulted in sanctions against their international affiliates were more egregious/deserving of sanction than cases against their domestic counterparts.

It’s equally possible that larger firms . In a response written to POGO in September 2019, former PCAOB spokesperson Torrie Matous, who is now an “Assurance Strategy Leader” at Deloitte, told POGO:

> “Not every inspection-related deficiency rises to such a level of severity that it should result in an enforcement investigation or the institution of an enforcement proceeding. **The PCAOB has finite resources available to it**. It therefore must *prioritize those resources* to focus on the activities that will allow it to accomplish its mission most effectively and efficiently. With respect to enforcement, this means that the PCAOB must prioritize carefully the matters it investigates and the cases it ultimately pursues.”

Elsewhere in that article, a former board member and general counsel, Lewis H. Ferguson, mentions that big firms have the capacity “to throw almost infinite resources” into their defense, while smaller firms were more likely to settle “because they really couldn’t bear the financial pressure of an extended investigation.” Higher level of enforcement actions are therefore not necessarily indicative of how the PCAOB spends most of its time, but they *could* signal the groups most likely to **settle** for sanctions.

This begs the question mentioned at the beginning of the article; **is the PCAOB looking out for ordinary investors?** While the Big Four’s average audit fail rate of 28.1% likely raises some concerns, there’s no evidence that any of these failed audits led to financial restatements, or were the result of deliberate malpractice. Of course, that evidence has been **deliberately** made unavailable. Congress *could* pass legislation making proceedings, hearings, and deliberations publicly available. They could also reveal company names on inspection reports, which would allow the public to track those companies and make better investor decisions. Such information could be used to track the long term effects of audit firms, their partnerships, the results of botched audits, and so much more. Beyond the fact that so much good data is sadly going unused, it also makes determining how the PCAOB operates extremely difficult.

Let me provide an example. In [May 2019](https://www.accountancydaily.co/kraft-heinz-admits-181m-accounting-misstatements), Kraft Heinz admitted to making \$181 million in accounting “misstatements” from 2015-2018, dropping their share price by 15% by the end of the month. PwC were their auditors, and have continued being their auditors since they were [relisted](https://www.investopedia.com/public-offering-as-hertz-relists-on-nasdaq-5209051) on the NASDAQ. On [September 3rd, 2021](https://www.sec.gov/news/press-release/2021-174), Kraft Heinz agreed to pay \$62 million to settle charges related to the scandal.

Now, was PwC responsible for these errors? While we can’t say for sure, let’s try and assess PwC’s involvement as a neutral investor.

Was 1 of the 38 audits that PwC failed from 2015-2018 belong to Kraft Heinz? There’s no way to tell. Did the PCAOB decide to review PWC’s audits after Kraft Heinz announced their financial restatement? There’s no way to tell. Did the PCAOB launch an investigation into their audit performance for the years in question? There’s no way to tell. Have there been hearings and legal proceedings regarding this case? There’s no way to tell. Is the PCAOB about to sanction PwC and/or their auditors? There’s no way to tell.

It’s *possible* that the PwC *may* have done something wrong, but the PCAOB, because of lack of resources, funds, motivation, or other reasons, ultimately never sanctioned them. Without these details, making comparisons between enforcement actions against firms is very difficult.

To put this in more simple terms,

This lack of information is universal across all firms, inspection reports, and most sanction reports. f

If we are trying to determine

In summation, we can definitively say that the PCAOB spends most of its time pursuing enforcement actions against smaller, domestic firms, and the auditors of those firms. When sanctions are pursued against the Big Four, monetary fines are much higher on average, though the majority have been against their international affiliates. Even then, the PCAOB pursues cases against domestic firms to such an extent that when comparing civic penalties between the Big Four and all other domestic firms, there is only a \$2.1 million gap between the two. This is significant, between domestic Big Four firms have much more impact on the economy comparatively.

This finding is consistent with statements made by former employees of the PCAOB.

Is the PCAOB targeting smaller firms and their auditors unfairly? Few details are provided in most reports that could be used to answer this question. Inspection reports are scarce in details, making it difficult to provide conclusive comparisons. This analysis is limited in answering this question, though if information into which firms *are* receiving failed audits, a more conclusive answer could be provided. Ultimately, though, there isn’t enough proof available to say if fines are pursued inequitably.

<br><br>

To switch over from an empirical analysis to the political, there is plenty to scrutinize over PCAOB enforcement, and I’d like to briefly go over some of those complaints.

The most glaring is the incredibly low amount of fines, especially considering they have the authority to fine firms up to \$15,000,000 for “reckless conduct” *or* “repeated instances of negligent conduct. The PCAOB has a very, *very*, **very** interesting interpretation of this section.

Ignoring the fact that the average fail rate among

Something that the PCAOB
