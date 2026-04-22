---
description: All you need to know about the SDS110 labs
icon: info
---

# General information

## Concept of the Lab Exercises

In these labs, you will immerse yourself in different Spatial Data Science scenarios that build on the lecture content and connect to real-world applications. Each lab begins with a short introduction to set the context, followed by a **task** (your mission) and a **workflow** (an example solution).

The goal is not to simply replicate the example provided, but to _adapt and apply the workflow to your own self-defined location, time frame, and topic of interest_. This transfer process will present challenges that often can’t be solved by following instructions alone. And that’s the point: it is precisely these moments of difficulty that facilitate learning and skill development.

The labs are designed to strengthen your practical understanding of core concepts such as spatial data management, visualization, and analysis using tools like [QGIS](./#why-qgis). You’ll work hands-on with real-world data to solve meaningful spatial problems and prepare for more advanced geospatial tasks ahead.

Lab sessions take place on-site. Please bring your own laptop so you can use a second screen. If you’d like to continue using QGIS beyond the lab or in other courses, [installing QGIS](https://qgis.org/download/) on your own machine is highly recommended (see [Getting Started](https://app.gitbook.com/o/y8IV2leCLZmUIh4iGtqg/s/iGEFlU3Y0YjkNsNoWM7s/) below).

Please note that we will not be providing any pre-processed data. You will be in control of your project from start to finish. Feel free to develop your own ideas as you go. We are happy to support and guide you along the way.

These Labs form part of the [SDS110 course](https://www.geo.uzh.ch/en/studying/sds_minor/30_minor.html) offered by the [Geography Department at UZH](https://www.geo.uzh.ch/en.html). All material is available under a CC-BY 4.0 license.

## Space and Time

Lab location: Y25-J-09/10 ([how to find us](https://www.geo.uzh.ch/en/units/rss/events/how-to-find-us.html))

Lab hours: Fridays 16:00 - 18:00&#x20;

## Error Culture

In these labs, **mistakes are** not just inevitable, they’re an **important** part of the learning process. By encountering and addressing errors, we deepen our understanding and develop practical problem-solving skills. **Questions** in the lectures, labs and via Teams **are** always **welcome**, so don’t hesitate to ask if something is unclear. Additionally, if you notice any mistakes or areas for improvement in these lab exercises, please let me know ([hendrik.wulf@geo.uzh.ch](mailto:hendrik.wulf@geo.uzh.ch)) your feedback helps me refine and improve the materials for everyone. Embrace the process, and remember: _learning happens through trying, failing, and improving_!

## Why QGIS?

We use [QGIS](https://qgis.org/) in this course because it is a powerful, open-source [geographic information system](https://youtu.be/UqDFc5eAwNg?feature=shared) (GIS) that allows everyone to explore, analyze, and visualize spatial data without licensing barriers. As one of the most widely used GIS platforms globally, QGIS combines professional-level capabilities, such as advanced cartography, spatial analysis, and support for diverse data formats, with an active development community and regular updates. QGIS is particularly well-suited for all education and professional settings, where reproducibility, accessibility, and long-term sustainability are key. By learning with QGIS, you gain practical skills in a tool used across science, government, and industry, while contributing to a broader culture of open geospatial science.&#x20;

These values are also financially supported by UZH's status as a Medium Sustaining Member of QGIS. Further information on the development, management and challenges of the QGIS project can be found in this recent publication "[The QGIS project: Spatial without compromise](https://www.sciencedirect.com/science/article/pii/S2666389925001138#sec3)".

Consider [installing QGIS](https://qgis.org/download/) on your personal computer so you can continue working on your projects beyond the lab sessions, this course, and even after your studies. We will be working with the latest long-term version of QGIS in the labs.

## AI in the Labs

AI tools such as large language models (LLMs) can be a great help in the labs. Whether you’re understanding spatial concepts, solving technical issues with QGIS or writing a lab report. UZH provides free access to [Copilot](https://www.zi.uzh.ch/de/staff/software-elearning/copilot.html) via [Microsoft Teams](https://www.zi.uzh.ch/de/students/software-elearning/microsoft.html). Other widely used models include Claude, ChatGPT, and Gemini. These tools can speed up your workflow, provide ideas, and help you better understand complex steps. For QGIS, the [Kue Assistant](https://buntinglabs.com/solutions/kue-ai) offers a domain-specific LLM directly within the software as a licensed plugin, making it especially tempting for GIS-related tasks. **However**, it’s important to stay actively involved in the learning process. If an AI tool “magically” solves a task without showing you how, you miss the opportunity to build your own understanding. _Use LLMs as smart assistants, not as shortcuts, to make your learning more efficient and reproducible._

## Communication

#### During Lab Sessions

You're always welcome to approach us directly during the lab. We’re happy to support you and answer questions, whether in English, Swiss German or German.

#### Outside of Labs

For questions related to the exercises, always use the _respective lab channels on_ [_Teams_](https://teams.microsoft.com/l/team/19%3AEb2HLy07-UauWIFPltYUxNqgrhubHaeO-mk__nvFlHM1%40thread.tacv2/conversations?groupId=5011d0cc-7bc6-4952-9fa3-5fffdf0f0473\&tenantId=c7e438db-e462-4c22-a90a-c358b16980b3). This way, everyone can jump in to help, and benefit from shared answers.&#x20;

> **Best practice:** When posting a question, describe your issue clearly and include:
>
> * A short description of what you tried to do
> * What went wrong or what you expected to happen
> * A screenshot of the problem (e.g. QGIS error message or map view)
> * The exact error message, if there is one

This will help everyone to understand your issue quickly and provide useful answers.

#### Personal matters

For anything that requires a private conversation feel free to pass by on Tuesdays and Wednesdays, 14:00–15:00 in room Y25-K-12 or email me: hendrik.wulf@geo.uzh.ch

## Submit your lab reports

Submit your lab report as a **PDF** via Teams by **12:00 (noon)** on the submission day.\
You’ll find a dedicated assignment in the Teams menu where you can upload your file. Please name your file exactly as follows: `Lab-X_first-name_last-name.pdf` (e.g. `Lab-3_Ken_Tucky.pdf`). Please make sure your report is complete and submitted on time.

<table data-header-hidden><thead><tr><th width="91.85003662109375">Week</th><th width="118.5999755859375">Date</th><th width="343.3499755859375">Labs</th><th>Submission</th></tr></thead><tbody><tr><td><strong>Week</strong></td><td><strong>Date</strong></td><td><strong>Labs</strong></td><td><strong>Submission</strong></td></tr><tr><td>1</td><td>19.09.</td><td>Lab 1: Visualizing Spatial Data</td><td> </td></tr><tr><td>2</td><td>26.09.</td><td> </td><td> </td></tr><tr><td>3</td><td>03.10.</td><td>Lab 2: Exploring Temporal Changes</td><td>Lab 1</td></tr><tr><td>4</td><td>10.10.</td><td> </td><td> </td></tr><tr><td>5</td><td>17.10.</td><td>Lab 3: Creating Online Maps</td><td>Lab 2</td></tr><tr><td>6</td><td>24.10.</td><td>Lab 4: Analyzing Spatial Data</td><td>Lab 3</td></tr><tr><td>7</td><td>31.10.</td><td> </td><td> </td></tr><tr><td>8</td><td>07.11.</td><td>Lab 5: Modelling Spatial Data</td><td>Lab 4</td></tr><tr><td>9</td><td>14.11.</td><td> </td><td> </td></tr><tr><td>10</td><td>21.11.</td><td></td><td></td></tr><tr><td>11</td><td>28.11.</td><td>Lab 6: Working with Sensitive Data</td><td>Lab 5</td></tr><tr><td>12</td><td>05.12.</td><td></td><td></td></tr><tr><td>13</td><td>12.12.</td><td> </td><td> </td></tr><tr><td>14</td><td>19.12.</td><td>No labs</td><td>Lab 6</td></tr></tbody></table>

I look forward to seeing your ideas take shape in your lab reports!

## Complete the labs successfully

* There are _6 lab exercises_, each worth _10 points._
* To pass the lab component, you need at least _36 points in total_ (60%).
* The labs contribute 50% of your final course grade (the other 50% comes from the final exam).
* Successfully completing the labs is _a prerequisite_ for taking the final exam.
* Optional tasks (“The Extra Mile”) can earn you _up to 1 bonus point_ per lab.
* Submit each lab on time and in the correct PDF format.&#x20;
* To keep everyone on track, late submissions without prior notice and explanation will be penalized: same day: –1 point, next day: –2 points, each additional day: –1 point per day
* Provide clear and concise answers in your own words.
* Include _captions_ for all figures and tables, and cite any external sources you use.
* You may write in _English or German_ – please be consistent within a report. English technical terms are welcome in German reports.

{% hint style="success" %}
**Tip**_:_ Create your own reusable template for Lab 1 that includes all required elements. This will save you time and help keep your reports consistent.
{% endhint %}

## Getting Started

### Software

There are three options for accessing QGIS. Choose the one that best fits your setup and preferences:

#### Option 1: Install QGIS locally on your Computer

If you prefer the most flexible option, you can install **QGIS** on your own machine.\
This option allows you to continue working on your project beyond lab sessions.\
You can [download QGIS](https://qgis.org/download/) from the official [QGIS website](https://qgis.org/).

> Tip: The "Long Term Release" (LTR) version is recommended for best stability.

#### Option 2: Use the GUIZ Computer Labs

The fastest way to get started is to log in with your UZH credentials on a computer in one of the GIUZ computer labs (rooms Y25-J-08/09/10). QGIS is already installed there.

#### Option 3: Use the Remote Desktop Server (RDS)

If you’re working from home or using your own device, you can connect (via [VPN](https://www.zi.uzh.ch/en/support/network/vpn_ISAC.html)) to a pre-configured Windows environment via the [Remote Desktop Server ](https://www.geo.uzh.ch/en/services/it-services/workplace.html)(RDS). This gives you full access to QGIS as if you were sitting in the lab.\
Setup instructions are available on the [GIUZ IT support](https://www.geo.uzh.ch/en/services/it-services.html) site.

### Lab Setup

To stay organized and ensure reproducibility, follow this setup for each lab:

1.  **Create a dedicated folder** on your working directory (locally or on the UZH server):\
    Example:

    ```
    /Users/[yourname]/[yourpath]/Lab_X/
    S:\course\sds110\stud\[yourname]\Lab_X\
    ```
2. **Use a clear subfolder structure**:
   * `01_qgis` → for your QGIS project files (.qgz)
   * `02_data` → for project data
     * `01_raw` → original datasets  (never edited)
     * `02_processed` → cleaned, clipped, projected data ready for analysis
     * `03_external` → background or reference data from third parties
   * `03_outputs` → for maps, tables, figures
   * `04_docs` → for documentation, reflections, and reports
3. **Open QGIS** (based on your chosen access option):
   * Personal device: Launch QGIS as installed
   * UZH lab computer: via Start Menu → “QGIS Desktop”
   * RDS: Open QGIS from the remote desktop environment

### Additional Tips

* Save your project frequently (`Project → Save`).
* Make regular backups of your files.
* When in doubt, ask! We are here to support you.

Once you’ve completed the setup, you’re ready to begin your spatial investigation!
