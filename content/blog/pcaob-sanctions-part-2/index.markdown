---
title: 'PCAOB Enforcement Actions (Part 2)'
excerpt: 'Reviewing over 500 sanctions and $38+ million in fines from the audit regulator.'
author: 'Nathan States'
date: '2022-12-01'
---

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

Unlike inspections, sanction reports have not changed much in content throughout the history of the PCAOB. Most enforcement actions are the result of one or multiple deficient audits, though a few involve other infractions. Some of the reports disclose the name of the company in question, which means it’s possible to track their outcomes. Given that most reports are unique, though, there’s not much consistent data to gather from each report outside of a few key variables.

[Section 1.04 of Sarbanes-Oxley](https://pcaobus.org/About/History/Documents/PDFs/Sarbanes_Oxley_Act_of_2002.pdf) describes the PCAOB’s ability to sanction firms / auditors as follows.

> If the Board finds, based on all of the facts and circumstances, that a registered public accounting firm or associated person thereof has engaged in any a ct or practice, or omitted to act, in violation of this Act \[…\] **the Board may impose such disciplinary or remedial sanctions as it determines appropriate**.

Sanctions can include censure, suspension from the accounting industry, limitation of financial activities, required additional education or training, and/or a monetary penalty. Fines are broken down into two categories; regular fines, which can range from;

> 1)  not more than **\$100,000** for a natural person or **\$2,000,000** for any other person; and
> 2)  in any case to which **paragraph (5)** applies, not more than **\$750,000** for a natural person or **\$15,000,000** for any other person;

Paragraph (5) refers to more serious violations, and are characterized by;

> 1)  **intentional or knowing conduct**, including *reckless conduct*, that results in violation of the applicable statutory, regulatory, or professional standard; *or*
> 2)  **repeated instances of negligent conduct**, each resulting in a violation of the applicable statutory, regulatory, or professional standard.

The PCAOB serves under the SEC, which means the Federal Civil Penalties Inflation Adjustment Improvements Act of 2015 also applies to the board. This [Act](https://www.sec.gov/rules/other/2022/33-11021.pdf) adjusts monetary fines for inflation, which means maximum penalties are actually about 40% higher than the amounts listed above.

### Quick Note About Data

When viewing the enforcement action database on [pcaobus.org](https://pcaobus.org/oversight/enforcement/enforcement-actions), you’ll notice that the total number of “settled disciplinary orders” i.e., sanctions is only *358* (as of 12/06/2022), but this dataset lists the total number at **516**. Most enforcement action are the result of single incidents in which a firm and multiple auditors get sanctioned, and the PCAOB will usually issue an individual report for each sanction. However, they sometimes will group incidents together, meaning some reports contain multiple sanctions. This has nothing to do with changing board members; for example, [this](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2022-033-kpmg-india.pdf?sfvrsn=3acddc01_4) incident contains two sanctions, while this incident has been broken down into [two](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2022-034-kpmg-sas.pdf?sfvrsn=173685c7_4) [separate](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2022-037-rodriguez.pdf?sfvrsn=9a174315_4) reports. Both were announced on the very same day.

Basically, the number listed on the PCAOB website doesn’t accurately reflect the total number of firms/auditors sanctioned, so reports that contain multiple sanctions have been broken down into individual rows.

## Overview

Breaking down sanctions can get hectic, so I decided to break them down into two different tables; one against *firms*, and one against *auditors.*

<br>

------------------------------------------------------------------------

<div class="most-width">

![](_images/table_enforcement_firm.png)

</div>

<br>

In case the table is hard to read, enforcement action has been broken down into two categories; sanctions against domestic, and sanctions against international, Both the total number of sanctions and the total amount in fines are included among the eight largest firms. Sole proprietors make up a sizable portion of the data, so they’ve been given they’re own special category, while all other sanctions have been combined into the group, “Everyone Else.”

For auditors;

------------------------------------------------------------------------

<div class="most-width">

![](_images/table_enforcement_auditors.png)

</div>

<br>

In total, **the PCAOB has sanctioned 516 firms/auditors for a total of \$38,177,000**. This includes both domestic and international firms. Considering that the board can impose a fine for each deficiency found during inspections, each fine can be up to \$2,000,000, and that the PCAOB has found thousands of deficiencies over the past 20 years, [some](https://www.reuters.com/article/us-usa-pcaob-enforcement/u-s-audit-regulator-levied-only-6-5-million-in-fines-in-16-years-study-idUSKCN1VQ2R0) [have](https://www.reuters.com/investigates/special-report/usa-accounting-PCAOB/) [argued](https://www.warren.senate.gov/imo/media/doc/Letter%20to%20Gensler%20on%20PCAOB.pdf) that the board is ineffective and weak (among other reasons). This post will try and examine these claims by breaking down some key features of the data.

## Enforcement Through Time

Before considering PCAOB sanctions as a whole, it should be noted that the number of sanctions given each year by the PCAOB has varied significantly throughout the board’s history, which seems to be directly connected to differences in enforcement philosophy between changing board members.

------------------------------------------------------------------------

<div class="box-width">

![](_images/chart_amount.png)

</div>

<br>

This is most evidenced by enforcement activity from 2015 - 2017, which saw the board sanction over 200 firms/auditors, comprising over 40% of total PCAOB sanctions. In [February 2018](https://www.complianceweek.com/sec-appoints-all-new-board-to-lead-pcaob-in-2018/8970.article), an entire new board was elected, and sanctions fell by about half the following year. In a PCAOB Webinar hosted for audit committee members, newly elected Chairman Bill Duhnke stated that the board had discovered from feedback that;

> “The PCAOB wasn’t necessarily playing well with others, that it was rarely receptive to feedback from stakeholders \[…\] **Consequently we’ve been actively trying to change that** \[…\]
>
> – Bill Duhnke, [quote at 1:38](https://youtu.be/it8ZNcqplBM?t=98)

The [Wall Street Journal](https://www.wsj.com/articles/sec-investigating-former-chair-of-auditing-industry-regulator-11623943373) would report in October 2019 that the board had “slowed its work amid board infighting, multiple senior staff departures, and allegations that the chairman has created a ‘sense of fear’ according to a \[May 2019\] whistle-blower letter and people familiar with the situation.” Dunkhe would later be investigated by the SEC for his handling of internal complaints and employee harassment, which eventually led Dunkhe and the entire board [getting canned in 2021](https://www.winston.com/en/capital-markets-and-securities-law-watch/sec-investigates-pcaob-chairman-and-removes-entire-board.html) (though that didn’t stop him from [returning to Capitol Hill](https://news.bloombergtax.com/financial-accounting/fired-audit-board-chief-returns-to-capitol-hill-as-senate-aide) as a Senate Aide, which is *niiice*).

Under the new leadership of Chairman Erica Williams, while the total number of sanctions has stayed relatively similar to previous years, they do differ in that nearly all of them involved a monetary penalty. Prior to 2015, only **35.51%** of sanctions involved a monetary fine; under Dunkhe, that number increased to **65.74%**. The only two sanctions not resulting in a monetary penalty under Williams have both explicitly stated in their [reports](https://pcaob-assets.azureedge.net/pcaob-dev/docs/default-source/enforcement/decisions/documents/105-2022-036-ramirez.pdf?sfvrsn=752cf0c0_4) that the board “would have imposed a civil money penalty of \$25,000” had they not “taken \[their\] financial resources into consideration.”

On a side note, while I creating this dataset, I didn’t record the other penalty types that may have occurred, such as if the firm/auditor was censured, whether the firm/auditor was suspended, if financial activities were limited, etc. All the reports do mention these explicitly, including early ones, so it is possible that data to see how punishments have changed over the years. In the future, I may return to collect this data.

Moving on, it should also be pointed out that the chart starts at 2005, or almost *three* years after the PCAOB was created. In an interview with the Project on Government Oversight (POGO), a “non-partisan independent watchdog,” a former PCAOB board member would tell them in [September 2019](https://www.pogo.org/investigation/2019/09/how-an-agency-youve-never-heard-of-is-leaving-the-economy-at-risk) that in the formative years of the PCAOB, some board members - including the original chairman - were philosophically opposed to aggressive enforcement. When the board did sanction a total of 5 firms and 7 auditors from 2005 - 2006, they chose not to impose a monetary fine on any of them. It wasn’t until December 2007 - over five years after the board was created - that the very first monetary fine was included within a sanction. That case involved **Deloitte**, currently the largest audit firm in the United States, and in this rare instance, the report includes the name of the company in question, which happened to be [Ligand Pharmaceuticals](https://www.nytimes.com/2007/12/11/business/11account.html).

In 2003, Ligand released a new painkiller and cancer medication treatment that sent the share price of the company soaring from \$4 in early 2003 to almost \$25 by April 2004. The audit partner in charge of the company was James Fazio, who had produced questionable work for the company in the past. His fellow Deloitte employees would ask him to step away from Ligand, and his supervisor even urged him to resign. Still, Deloitte would continue to allow Fazio to work with Lignad.

One of the reasons why Ligand saw such massive stock gains is because they generously allowed buyers (including wholesalers) to refund unsold drugs to the company. The auditors created a formula to estimate the total number of returns, but the PCAOB would later find that Deloitte had not challenged Fazio’s estimates despite opposing evidence that return rates were actually much higher. After the share price dropped during the summer of 2004, Ligand would hire new auditors, who immediately restated their 2003 earnings, saying that **more than half the revenue they reported that year should not have been reported**. This sent the share price crashing back down all the way to \$4, and it would take them until 2017 - an entire **thirteen years** later - to reach their peak of \$28 again.

Deloitte was fined \$1 million.

Fazio and Deloitte agreed to the sanctions without admitting wrongdoing, but for practical purposes, what they committed in this case was essentially a pump and dump scheme, and it hurts more than just investors. The company immediately downsized, causing hundreds to lose their jobs, and any potential 401(k)s that included matching would of taken massive hits. While most deficient audits do not result in these outcomes, there’s always a portion that do, and their effects are disastrous for all involved (besides those in on the scheme, of course). While early iterations of the board only pursued penalties in the most severe cases, it’s clear this philosophy has changed since.

## Sanctions Against the Big Four

The four largest firms audit almost half of all US filers, including nearly all the companies on the S&P 500, which means they have significant influence on overall audit and financial quality in the United States. Considering that the PCAOB has inspected a similar number of audits between the big four and all other domestic firms, we’d expect each group to receive similar discipline. This obviously isn’t the case; the PCAOB has sanctioned domestic big four firms just **five** times for **\$6,500,000** in its 20+ year existence. Domestic firms outside the eight largest have been sanctioned 123 times comparatively, or **2,400%** more. While fines against the big four are much “harsher” on average, they also rarely seem to get punished at all.

I put “harsher” in quotations, because while their fine amounts might be higher on average, that doesn’t necessarily make it harsher when factoring in their higher pay. In 2018, Deloitte was fined \$500,000 for its audit work on **Jack Henry & Associates**. Deloitte produced deficient work on three consecutive audits from 2012 - 2014, which led to the company restating their financial statements in June 2015. This didn’t lead to a noticeable drop in share price; in fact, the company would finish the year +8%. I also couldn’t find any details on Jack Henry downsizing, so it’s difficult to know the cause and effect of these restatements. Ultimately, though, beyond the fact that the PCAOB chose not to invoke Paragraph (5) in a case where a firm failed three audit inspections in a row within the same company, which I’d argue fits the “repeated instances of negligent conduct” part very well, they *still* let Deloitte off profitable. Despite producing literally useless work, Deloitte was still paid \$3,877,245 for the audits. So it’s not very accurate to say their fines are “harsher,” only that they tend to be for higher amounts.

Interestingly, Deloitte is the best performing firm in terms of deficiency rates (as disclosed in inspection reports, check out my post [here](https://www.nathanstates.com/blog/pcaob-inspections-part-1/)) among other domestic affiliates, but they’ve received the most sanctions for the most amount. KPMG, the worst performing firm among the big four, has **never** been sanctioned, which is quite amazing beyond their poor performance. Not only did they post a deficiency rate of almost 50% from 2014 - 2017, the highest four year span among the big four, they were also fined **\$50 million** [in 2019](https://www.sec.gov/news/press-release/2018-6) by the SEC for attempting to steal confidential information from a former PCAOB employee to fraudulently improve their inspection results. An argument could be made that failing half your audit inspections, and then attempting to cheat them instead of actually improving, would also fall under Paragraph (5) of “repeated and negligent conduct.” **Clearly, inspection reports tell us little to nothing about who the PCAOB chooses to sanction**.

On the other hand, enforcement action against *international* affiliates looks very different. Among the big four, **32** international firms have been sanctioned compared to only 5 domestic, and international has been fined more than *double* that of domestic firms.

------------------------------------------------------------------------

<div class="box-width">

![](_images/chart_country.png)

</div>

Deloitte has again been fined the most, which is largely skewed because of an \$8 million fine regarding its Brazilian affiliate that saw 12 auditors being barred permanently from the industry. According to the PCAOB report, *Deloitte Brazil* [knowingly issued false audit reports](https://pcaobus.org/news-events/news-releases/news-release-detail/pcaob-announces-8-million-settlement-with-deloitte-brazil-for-violations-including-issuing-materially-false-audit-reports-and-12-individuals-also-sanctioned-for-various-violations_601) in 2010 for *Gol Linhas Aéreas Inteligentes* (**NYSE**: GOL), one of Brazil’s largest airlines. When the company announced a financial restatement in August 2011, the share price dropped more than 65%, prompting a PCAOB investigation. When the board asked for documentation from *Deloitte Brazil*, senior managers “attempted to cover up its violations by improperly altering documents in connection with a 2012 PCAOB inspection” and by “obstructing a subsequent investigation.”

To this day, it remains only the second time the PCAOB has invoked Paragraph (5); the other happened very recently. On 12/06/2022, the [PCAOB announced](https://pcaobus.org/news-events/news-releases/news-release-detail/imposing-7-7-million-in-fines-pcaob-sanctions-three-firms-four-individuals-kpmg-global-network) it was sanctioning three international *KPMG* affiliates for \$7.7 million, the largest being a \$4 million fine against *KPMG Colombia* when they tried to alter audit documentation in anticipation of a PCAOB inspection. However, no company name is disclosed in any of the three reports, so there’s no way to know the outcomes of these incidents, assuming there were any.

In terms of inspection reports, Colombian affiliate have recorded a 24% deficiency rate, much lower than the average of around 34%. Canada is the most inspected international country, plus has one of the worst deficiency rates at 45%, but has received less than \$1.8 million in fines. Similar to comparisons between domestic firms, there’s little to any correlation between performance on inspection reports and the number/amount a firm will be sanctioned.

## Sole Propeitors and Small Firms

While more attention has been given to international big four firms compared to domestic, sole proprietors have received the most sanctions of any group at 71. If you factor in the additional 142 sanctions against domestic auditors of firms outside of the largest eight, which many are co-partnerships or under 10 employees, the majority of sanctions have been against small firms/auditors.

------------------------------------------------------------------------

<div class="box-width">

![](_images/chart_proprietors.png)

</div>

<br>

Auditing companies as a single owned business, or even a small firm, requires some significant credibility, so these are not necessarily small, mom-and-pop shops that are being hurt. Many of these cases involve pump-and-dump schemes on penny stocks, where a few auditors with presumably something to gain will sign off on deficient audits to artificially boost the share price. Still, this reflects the PCAOB’s decision to pursue cases against smaller, more inconsequential firms, as opposed to much larger ones like Crowe or RSM, who have been fined collectively much less.

### Understanding PCAOB Sanctions

This might be more than just board philosophy, though. Referring back to the [POGO article](https://www.pogo.org/investigation/2019/09/how-an-agency-youve-never-heard-of-is-leaving-the-economy-at-risk), several statements from former PCAOB employees would seem to suggest that their pursuit of sanctions against sole proprietors and small firms is a *budgetary* decision. Former board member Lewis Ferguson stated that big firms had the capacity “to throw almost infinite resources” into their defense, while smaller firms were more likely to settle because they can’t “bear the financial pressure of an extended investigation.” Torrie Matous, then PCAOB spokesperson, said in an email to POGO;

> Not every inspection-related deficiency rises to such a level of severity that it should result in an enforcement investigation or the institution of an enforcement proceeding. **The PCAOB has finite resources avaliable to it. It therefore must prioritize these resources to focus on the activities that will allow it to accomplish its mission most effectively and efficiently**.”

Matous had previously served on the Chief of Staff to Martha Roby (R-AL) before working for the PCAOB. Most staff and senior employees come from political backgrounds, which isn’t surprising, because regulating any industry is inherently political. Needless to say, though, politics undoubtedly plays a role in enforcement, and it’s not difficult to see different idealogies fall.

Specifically, in 2010, the case **Free Enterprise Fund v. Public Company Accounting Oversight Board** was brought to the Supreme Court that challenged the constitutionality of the PCAOB. The Free Enterprise Fund was an offshoot of *Club for Growth*, and was led by Stephen Moore. For those reading who may be unaware what either of those things are, without getting too bogged down in the details, *Club for Growth* is a 501(c)(4) nonprofit “conservative” organization, with two separate political action committees (or PACs) that help fund the campaigns of aspiring “conservative” politicians. And when I say “conservative,” I mean [giving \$20 million to Republican lawmakers in 2018 & 2020 who would later vote against certifying the results of the 2020 election](https://www.theguardian.com/us-news/2021/jan/15/trump-republicans-election-defeat-club-for-growth), i.e., not actually conservative. Basically, the people who want to overturn elections are the same people who want to destroy audit regulations, which really shouldn’t be surprising.

Stephen Moore, on the other hand, is a longtime Republican operative who, in recent years, helped write the Tax Cuts and Jobs Act of 2017, which lowered the corporate tax rate from 35% to 21%. Moore claimed that these cuts were “[paying for itself](https://www.wsj.com/articles/the-corporate-tax-cut-is-paying-for-itself-1537310846),” a statement that is [empirically false](https://www.brookings.edu/policy2020/votervital/did-the-2017-tax-cut-the-tax-cuts-and-jobs-act-pay-for-itself/). He also believes climate change is “[not settled science](https://www.washingtontimes.com/news/2015/mar/15/stephen-moore-climate-change-not-settled-science/),” so you can imagine he doesn’t favor regulation. In 2020, the Trump administration announced a proposal that would dissolve the responsibilities of the PCAOB into the SEC, which undoubtedly would of severely wakened audit regulations, though obviously it did not go through.

Anyways, transitioning back to the Supreme Court case, while they did uphold the constitutionality of the board, they did declare that the provision that PCAOB board members could only be removed for “just cause” was unconstitutional because it offered them a “dual-layer” of protection (while SEC members can be removed by the President, PCAOB board members are elected by the SEC, meaning the President can’t control them). Regardless of the constitutional soundness of this decision, the practical effect it has can already be observed; entire boards are routinely replaced following the election of new administrations. Both Republicans and Democrats have, unsurprisingly, complained of the board’s increasing “politicization” (as if regulating financial industries, or any economic industries for that matter, isn’t inherently political). It’s evident that while Democrats support continuing the PCAOB, Republicans have been pushing to limit the board or dissolve it all together. Needless to say, this will have massive impact on PCAOB enforcement action.

There is one last consideration, and that’s the *revolving door*, or the process in which government regulators eventually work for the companies who they previously regulated. Another report by [POGO](https://www.pogo.org/press/release/2020/new-pogo-report-illustrates-extent-of-revolving-door-between-accounting-firms-and-their-regulator) found that at least 40% of PCAOB employees had worked at one of the four largest audit firms, and most PCAOB employees do eventually return to the private industry. The idea that regulators may soften discipline to appease potential future employers is not at all unique to the PCAOB, though it isn’t clear to what degree (if any) it affects how the board decides enforcement.

All this is to say that **analyzing PCAOB enforcement from a purely data perspective fails to adequately describe how the board pursues sanctions in any meaningful way**. Sanctions make so little sense that the only way to try and understand them is to consider other factors. Even then, it isn’t clear how the board ultimately decides enforcement, and everyone will have differing opinions on how sanctions will change depending on those factors.

## Main Takeaways

- In total, **the PCAOB has sanctioned 516 firms/auditors for a total of \$38,177,000**. This includes both domestic and international firms.
- International affiliates of the big four have been sanctioned **five times** more often for over *triple* the amount of their domestic counterparts.  
- Domestic sole proprietors have seen significant attention from the PCAOB, being fined 71 times for \$563,000. This is more than domestic KPMG, BDO, RSM, and Crowe firms have been fined *combined* (by **\$548,000**\*).
- Enforcement has changed significantly from year to year, and this seems dependent on a combination of board philosophy, politics, and/or budgetary concerns. Enforcement does **not** seem to be at all strongly connected to performance on inspection reports.

<br><br><br><br><br><br><br><br><br><br>

<div class="small-width">

<div id="htmlwidget-1" style="width:100%;height:500px;" class="echarts4r html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"theme":"","tl":false,"draw":true,"renderer":"canvas","events":[{"data":{"type":"legendUnSelect","name":"hp"}}],"buttons":[],"opts":{"yAxis":[{"show":true}],"xAxis":[{"type":"value"}],"legend":{"data":["hp"]},"series":[{"data":[{"value":[10.4,205]},{"value":[10.4,215]},{"value":[13.3,245]},{"value":[14.3,245]},{"value":[14.7,230]},{"value":[15,335]},{"value":[15.2,180]},{"value":[15.2,150]},{"value":[15.5,150]},{"value":[15.8,264]},{"value":[16.4,180]},{"value":[17.3,180]},{"value":[17.8,123]},{"value":[18.1,105]},{"value":[18.7,175]},{"value":[19.2,123]},{"value":[19.2,175]},{"value":[19.7,175]},{"value":[21,110]},{"value":[21,110]},{"value":[21.4,110]},{"value":[21.4,109]},{"value":[21.5,97]},{"value":[22.8,93]},{"value":[22.8,95]},{"value":[24.4,62]},{"value":[26,91]},{"value":[27.3,66]},{"value":[30.4,52]},{"value":[30.4,113]},{"value":[32.4,66]},{"value":[33.9,65]}],"yAxisIndex":0,"xAxisIndex":0,"name":"hp","type":"line","coordinateSystem":"cartesian2d"}],"tooltip":{"trigger":"item"}},"dispose":true},"evals":[],"jsHooks":[]}</script>

</div>
