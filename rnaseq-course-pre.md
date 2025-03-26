---
title: "RNAseq Data Processing & Analysing Course"
author: "Alex Gibbs"
#full-width: true
---

<br>

# Pre-course tasks

<br>

---
## Install Cardiff Virtual Private Network (VPN) - Global Protect
---
---

- We will need to have the Global Protect VPN installed on our PC's in order to connect to HAWK off campus.

<b>For MAC</b>

- Use this [link](https://software.cardiff.ac.uk) and log in to the Software Downloads website with your university log in.
- Then click on *Global Protect*, then click the *MAC OS* link. Click on the *GlobalProtect-6.2.4.pkg* download button to initiate the download to your PC.
- Once the file has downloaded, navigate to it and double-click on it to initiate installation onto your PC.
- Navigate through the installer by clicking *Continue*.
- Check boxes for *GlobalProtect* and *GlobalProtect System Extensions* and click *Continue*.
- Click *Install*.
- Enter your admin password when prompted and click *Install Software*.
- Click *Close* once the installation has completed.
- When prompted, choose to *Allow* the system extension in Security and Privacy preferences.
- Click on the system notification to Allow notifications from GlobalProtect.

<b>For Windows</b>

- Use this [link](https://software.cardiff.ac.uk) and log in to the Software Downloads website with your university log in.
- Then click on *Global Protect*, then click the *Windows* link, then click on the 32- or 64-bit client link. To find out what bit your system is, go to Control Panel > System and Security > System. In the System area, the System Type shows if the system is 32 bit or 64 bit.
- Now click on the *GlobalProtect64-6.2.4.msi* download button to initiate the download to your PC.
- Click *Run* to start installation. Accept any browser security messages if they pop up.
- Click *Next* in the setup wizard and accept the default installation folder on your PC.
- Click *Next* to install the Agent in the default location in *Program Files*.
- Click *Next* to continue with the installation.
- You will be notified once installation has completed, click *Close* to finish.

<b>For Linux</b>

- Use this [link](https://software.cardiff.ac.uk) and log in to the Software Downloads website with your university log in.
- Then click on *Global Protect*, then click the *Linux* link.
- Follow the instructions from there.

<br>

**Connecting to the VPN:**

- The VPN runs automatically after installation.
- Enter the portal address as *ras.cf.ac.uk*
- Enter your university username and password.
- Click *OK* when prompted to allow GlobalProtect access to your Desktop, Documents, and Downloads folders (three prompts).
- To disconnect, click on the GlobalProtect icon and click 'Disconnect'.
- To reconnect, click on the GlobalProtect icon and click 'Connect'.

<br>

---
## Install Visual Studio Code (VSCode)
---
---

- For this course, we will be using VSCode for everything.
- VSCode is an Integrated Development Environment (IDE) for code development. It is essentially a customisable notebook for coders.
- The beauty of this tool is that we can install extensions onto the app which means we can do everything in one window.

<u><b>To install VSCode</b></u>
- First, head on over to [the download page](https://code.visualstudio.com/Download).
- Select the version for your operating system, download and install the software.

- <b>Windows Users</b>: navigate to the location where you downloaded the file, right click on it, and click 'Run as administator'. This should avoid any problems with your antivirus software.

- <b>Note:</b> I am not sure how well this app works for Linux users.

- Once the app has downloaded, it should automatically open.
- Exit the startup page by clicking on the 'X' on the tab at the top of the window.

<br>

---
---
#### Install tools and packages in VSCode
---

- Now that we have VSCode installed, we can now install the extensions needed for the whole course.

<u><b>Remote - SSH</b></u>
- This extension allows us to connect remotely to HAWK through a terminal window within the software.

- To install, click on the 'Extensions' button.
- Search for 'remote ssh'.
- Click on the 'Remote - SSH' extension and then 'Install' at the top.

<img src="/assets/img/pre-1.png" alt="install Remote-SSH" width="1000"/>

- Then, to setup the extension to login to your HAWK, click on 'Remote Explorer'.
- Then click on the '+' symbol to add our SSH target.
- You will see a drop down box appear at the top of the screen asking for your login details.
- Enter the following (add your username) and hit enter:

```
ssh c.c1234567@hawklogin.cf.ac.uk
```

- You will then be asked to select a SSH configuration file to update.
- Select the **top** one.

<img src="/assets/img/pre-2.png" alt="Setting up HAWK" width="1000"/>

- You will now see the host has been added to the 'Remote Explorer' window.

<br>

---

<u><b>Optional: Excel Viewer</b></u>
- These are not required, but can massively help when editing CSV files.
- Excel Viewer allows us to open CSV files as a table, rather than a text file.
- Click 'Extensions' button.
- Search for 'excel viewer', then 'Install' at the top.
- To open CSV files with this extension, right click on the file and click 'Open With...'. This opens a drop down menu at the top of the window.
- Click on the second option: 'CSV Editor'.

<img src="/assets/img/pre-5.png" alt="Installing Excel Viewer" width="1000"/>

<br>

---
## Connecting to HAWK
---
---
- To connect to HAWK, click on the 'Remote Explorer' button, then click 'Connect to Host in New Window' button. This opens a new VSCode window with the remote host.
- You will be prompted (top box) to enter the host operating platform.
- Click **Linux**.
- Then you will be prompted to continue, click **Yes**.
- Then you will be asked for your password. Do this and hit enter.
- You are now connected to HAWK.

<img src="/assets/img/pre-3.png" alt="Connecting to HAWK" width="1000"/>

<br>

---
## Download Gene Set Enrichment Analysis (GSEA) Application
---
---

- GSEA application is needed at the end of the course to analyse our DEGs.
- Go to the following [webpage and download GSEA version 4.3.2](https://data.broadinstitute.org/gsea-msigdb/gsea/software/desktop/4.3/) (latest version may not work for everyone so we are using this one).
- Make sure you click on the download relevant to your operating system: GSEA_MacApp for Mac, GSEA_Win for Windows, GSEA_Linux for Linux. **Make sure you download the installer (.exe for Windows)**
- Unzip the file by double clicking or right-clicking and selecting unzip. Windows users: you may need to run as administrator if you are getting any problems with your antivirus software.
- Now you should have, in the same location, your `GSEA_4.3.2.app`.
- You can move this app to anywhere you want and it will still work when you open it.
- Double-click the app to ensure that it is working correctly.

<img src="/assets/img/figure-48.png" alt="GSEA window" width="1000"/>


<br>

---
## Learning Materials
---
---

- We won't be covering how RNAseq technologies work or the specific details of the nf-core pipelines.
- Please see the following links to learning materials to cover them:

**Bulk RNA Sequencing**

- [Beginners guide to bulk RNA-seq analysis](https://www.youtube.com/watch?v=WW94W-DBf2U)
- [How Illumina sample prep and sequencing works](https://www.youtube.com/watch?app=desktop&v=fCd6B5HRaZ8)
- [A Gentle Introduction to: RNA-seq](https://www.youtube.com/watch?v=tlf6wYJrwKY)

**nf-core Pipelines**
- Details of the fetchngs pipeline, including what tools/packages etc are used can be found [here](https://nf-co.re/fetchngs/1.11.0).
- Details of the rnaseq pipeline, including what tools/packages etc are used can be found [here](https://nf-co.re/rnaseq/3.14.0/). Also a [YouTube video](https://www.youtube.com/watch?v=qMuUt8oVhHw) briefly explaining the pipeline.
- Details of the differentialabundance pipeline, including what tools/packages etc are used can be found [here](https://nf-co.re/differentialabundance/1.4.0).

**GSEA**
- [GSEA User Guide](https://www.gsea-msigdb.org/gsea/doc/GSEAUserGuideFrame.html?xtools_gsea_GseaPreranked). Scroll down for info on how to interpret the data.
- [GSEAPreranked Information Page](https://www.gsea-msigdb.org/gsea/doc/GSEAUserGuideFrame.html?xtools_gsea_GseaPreranked).

**g:Profiler**
- [g:Profiler Docs page](https://biit.cs.ut.ee/gprofiler/page/docs)

**EnrichR**
- Link to the Enrichr-KG [tutorial page](https://maayanlab.cloud/enrichr-kg/tutorial).

**Rummagene**
- Link to the [paper describing the tools and implemented stats](https://www.nature.com/articles/s42003-024-06177-7)

**RummaGEO**
- Link to the [User Manual](https://rummageo.com/usermanual)

**Ingenuity Pathway Analysis (IPA)**
- Link to the [Help Page](https://qiagen.my.salesforce-sites.com/KnowledgeBase/KnowledgeNavigatorPage?categoryName=IPA)
