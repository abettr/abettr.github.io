---
title: "An introduction to Unix, NF-core & FASTQ prefetch"
author: "Aimee Bettridge, Alex Gibbs"
editors: "Alex Gibbs, Robert Andrews"
toc: true
toc_sticky: true
#full-width: true
---

<style>
  hr {
    box-sizing: border-box;
    width: 100%;
    margin: 0;
  }
</style>

<hr style="border: 4px double #3E1628; background-color: #7B1F3F; height: 1px; width: 100%; margin: 0; border-radius: 10px;">
<h1 id="day1" style="font-size: 42px;">Day 1</h1>

*Written by:* *{{ page.author }}*  
*Editors:* *{{ page.editors }}*

<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## *i.* Course goal
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

Hello and welcome to our bulk RNA-seq analysis course! This course aims to equip you with the foundational skills and knowledge required to set up a supercomputing environment and analyse a bulk RNA-seq dataset from FASTQ prefetch to identifying genes of interest. We will use a simple public data set to demonstrate how to do this. At the end of this course, we will offer our support in setting up your own custom analysis pipeline. We strongly encourage you to attend each session as it may be difficult to catch up later.
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## *ii.* Today's Learning objectives
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

You should expect to:
- Set up a remote session using your laptop.
- Navigate and use the Hawk supercomputing environment.
- Call basic Unix commands with working examples.
- Download raw bulk RNA-seq data from a public repository.
- Reproduce today's analysis with own data.
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## *iii.* What we will not do here
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- Cover how RNA-seq libraries are generated and next generation sequencing (NGS). Watch this **[video](https://www.youtube.com/watch?v=tlf6wYJrwKY)** for an overview.
- Cover more advanced sequencing technologies (e.g. scRNA-seq). We also provide training in scRNA-seq analysis.
- Overburden you with too much information; we will signpost towards further reading and support where appropriate.
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## BASH scripting
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

* <font color="red">BASH scripts are ultimately not an efficient method of storing a workflow.</font> 
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## FASTQ format
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

A placeholder for FASTQ file format.
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

# 2. An introduction to nf-core
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;"> 

- A community-led framework which hosts a curated set of open-source analysis **[pipelines](https://nf-co.re/pipelines/)** built using Nextflow.
- Includes >100 maintained pipelines and >1400 reusable modules that aim  to be accessible, well-documented, and easily understandable.
  - Adheres to **[FAIR](https://fellowship.elixiruknode.org/latest/carpentries-course-fair-pointers)** principles. 
- Its ethos encompasses open-sourced, collaborative and supportive principles. Their seminars and training are free to join and they offer further support through their **[slack](https://nf-co.re/join)** channel.
- **We will be using up-to-date nf-core pipelines for our analysis**. 
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## 2.1. What is nextflow?
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

Nextflow is Both a workflow management system and a domain-speciifc language based on Java and Groovy, meaning it:
  - Allows us to execute increasingly more complex tasks in the age of big data.
  - Runs several different tasks on Hawk automatically with minimal input. Convenient!
  - Organises our tasks through channels, allowing them to communicate with each other.
<br>

*Channels commonly convey data inpts and outputs between processes to get to the final product:*

<img src="/assets/img/AB_Nextflow.png" alt="A simple Nextflow workflow example"
     style="display: block; margin: 0 auto;" width="600" />
<br>

<details style="font-family: Arial, sans-serif; margin-bottom: 0; color: #250D18;">
  <summary style="color: #DEABA0; border: 2px solid #DEABA0; border-radius: 6px; padding: 0.5em; margin-bottom: 1.5em; cursor: pointer; font-weight: bold; background-color: #250D18;">
    Nextflow boasts many other benefits...
  </summary>

  <div style="border: 2px solid #DEABA0; border-radius: 6px; background-color: #250D18; padding: 0em; color: #DEABA0; margin-top: -2em; /* pulls it closer to the summary */ transition: margin-bottom 1.5">
    <ul style="margin: 1em 0 0 0; padding-left: 2em;">
      <li><em>Each task can be written in virtually any coding language.</em></li>
      <li><em>Is portable (compatible across platforms and job schedulers on the supercomputer).</em></li>
      <li><em>Is reproducible through containerisation and version control.</em></li>
      <li><em>Is highly customisable.</em></li>
      <li><em>Easily resume work from any checkpoint.</em></li>
      <li><em>Parallel processing – performs many tasks independently and simultaneously.</em></li>
    </ul>
  </div>
</details>

<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px; margin-top: 1.5;">

<!-- Add the following CSS -->
<style>
  details[open] > div {
    margin-bottom: 1.5em;  /* Increase margin-bottom when expanded */
  }
</style>

### 2.1.1. What you should know
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

<font color="red">ADD SOMETHING HERE ABOUT CONFIURATION AND PARAMETERS</font>

Everytime you execute or resume Nextflow a unique work directory based on a hash is created. This work directory is used to stage input files and scripts in addition to writing outputs for downstream tasks and/or publishing (desired outputs are copied over to our project input and output directories).

You will find the Nextflow logfile and work directory from wherever you execute your nf-core pipeline. For example:

```bash
$ tree -a work
work
└── 39
    └── ef16fd4a3de2f512ed91a85ce900bf
        ├── .command.sh
        ├── .command.out
        ├── .command.err
        ├── .command.log
        ├── R1.fastq
        └── R2.fastq
```
reveals a very simplified expected Nextflow work directory structure containing final and/or intermediate data outputs and error files.
<br>
<details style="font-family: Arial, sans-serif; margin-bottom: 0; color: #250D18;">
  <summary style="color: #DEABA0; border: 2px solid #DEABA0; border-radius: 6px; padding: 0.5em; margin-bottom: 0em; cursor: pointer; font-weight: bold; background-color: #250D18;">
    How to navigate these hidden files
  </summary>

  <div style="border: 2px solid #DEABA0; border-radius: 6px; background-color: #250D18; padding: 0em; color: #DEABA0; margin-top: -0.5em; /* pulls it closer to the summary */ transition: margin-bottom 1.5">
    <table style="width: 100%; border-collapse: separate; border-spacing: 0; color: #DEABA0; border: 2px solid #DEABA0; border-radius: 6px;">
      <thead>
        <tr>
          <th style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">File name</th>
          <th style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Description</th>
          <th style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Useful how?</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">.command.sh</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Script Nextflow uses to run a task</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Easier to spot syntax/variable issues</td>
        </tr>
        <tr>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">.command.out</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Process standard output</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Check if process completed successfully</td>
        </tr>
        <tr>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">.command.err</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Process standard error</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">For reading warnings, errors, and debugging</td>
        </tr>
        <tr>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">.command.log</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Combined standard output & error</td>
          <td style="font-weight: bold; border-bottom: 1px solid #DEABA0; border-top: 1px solid #DEABA0; border-left: 1px solid #DEABA0; border-right: 1px solid #DEABA0; background-color: #250D18; padding: 0.5em;">Get a full picture of what went wrong</td>
        </tr>
      </tbody>
    </table>
  </div>
</details>

The nextflow logfile (.nextflow.log) is handy to view the total pipeline execution history and can shed light on potential errors, resource usage, time to completion and task submission details.

<div style="position: relative; margin-top: 2rem;">
  <div style="background-color:#250D18; border-top: 37px solid #7B1F3F; padding: 10px 16px 10px; color: #DEABA0; position: relative;">
    <span style="position: absolute; top: -30px; left: 1px; font-weight: bold; color: #DEABA0; background-color: #7B1F3F; padding: 5px 10px; line-height: 20px; display: inline-block; border-radius: 4px;">
      &#128221; Note:
    </span>
    Learning Nextflow from scratch can be daunting. We will not go into unnecessary detail here, though there are several good resources and training opportunities we recommend through 
    <a href="https://arcca.github.io/index.html#register-for-future-courses" style="color: #019E95; text-decoration: underline;">ARCCA</a>, <a href="https://nf-co.re/events/training" style="color:  #019E95; text-decoration: underline;">nf-core</a>, and 
    <a href="https://www.nextflow.io/docs/latest/index.html" style="color: #019E95; text-decoration: underline;">Nextflow</a> documents.
  </div>
</div>

<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px; margin-top: 2rem;">

# 3. nf-core/fetchngs Pipeline
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- This **[pipeline](https://nf-co.re/fetchngs/1.12.0/)** allows us to fetch metadata and raw FASTQ files from public and private databases.
- The pipeline currently supports SRA/ENA/DDBJ/GEO/Synapse IDs.
- **You only need to input a list of sequence accessions (sequence database IDs) one per line**.

<img src="/assets/img/fetchngs_AB.png" alt="fetchngs pipeline workflow, from FASTQ prefetch to downstream pipelines."
     style="display: block; margin: 0 auto;" width="1000" />

*These accessions are used to grab metadata and then download FASTQ files before collating into a single samplesheet for the nf-core rnaseq pipeline.*

<details style="font-family: Arial, sans-serif; margin-bottom: 0; color: #250D18;">
  <summary style="color: #DEABA0; border: 2px solid #DEABA0; border-radius: 6px; padding: 0.5em; margin-bottom: 1.5em; cursor: pointer; font-weight: bold; background-color: #250D18;">
    For a more in-depth explanation...
  </summary>

  <div style="border: 2px solid #DEABA0; border-radius: 6px; background-color: #250D18; padding: 0em; color: #DEABA0; margin-top: -2em;">
    <ol style="margin: 1em 0 0 0; padding-left: 2em;">
      <li><em>Your accessions are checked against existing databases and if they are compatible with the ENA API.</em></li>
      <li><em>Accession metadata is collected through ENA API.</em></li>
      <li><em>FASTQ files are downloaded (default = ftp) and md5 checksums are generated.</em></li>
      <li><em>Metadata and FASTQ file paths (locations) are collated into a samplesheet for the rnaseq pipeline.</em></li>
    </ol>
    <div style="padding-left: 2em; margin-top: 1em;">
      💡 If FASTQ prefetch does not work, or you prefer another download tool, add <code style="background-color: #7B1F3F; color: #DE7B21;">--download_method</code> parameter to your command.  
      <br>
      💡 Md5 checksums are 32-character alphanumeric hashes that are used to check if your files may have corrupted during transfer and is a handy troubleshooting tool.
      <br>
      <div class="mb-4"></div>
    </div>
  </div>
</details>

<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## 3.1. How to find a public dataset
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- We first need to find a dataset. Some of you may already have found one via a paper that you have read etc.
- There are multiple repositories that we can find samples on. The two most common are **Gene Expression Omnibus ([GEO](https://www.ncbi.nlm.nih.gov/geo/))** and **[Array Express](https://www.ebi.ac.uk/biostudies/arrayexpress)**.
- **For this course, we will use GEO to find our dataset.**
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

### 3.1.1. Navigating GEO
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

First, visit https://www.ncbi.nlm.nih.gov/geo/ and follow the video/written instructions below to locate your working GEO dataset.

**This dataset will enable you to compare effects of hypoxia on gene expression across two different cell lines.**

<video width="100%" controls loop="" muted="" autoplay="">
  <source src="/assets/img/GEO_dataset.mp4" alt="How to find workshop dataset">
</video>
<br>
<br>
To summarise, you:

<ol style="margin: 1em 0 0 0; padding-left: 2em;">
  <li><em>Type 'renal cell carcinoma rna seq hypoxia' in the search bar.</em></li>
  <li><em>Click on the link presented in the pop up window showing all results.</em></li>
  <li><em>Scroll down and click on 'Effect of hypoxia on gene expression among HK-2 cells and 786-0 cells'.</em></li>
  <li><em>Can find what you just did through <strong><a href="https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE225253">here</a></strong>.</em></li>
</ol>

<div style="position: relative; margin-top: 2rem;">
  <div style="background-color:#250D18; border-top: 37px solid #7B1F3F; padding: 10px 16px 10px; color: #DEABA0; position: relative;">
    <span style="position: absolute; top: -30px; left: 1px; font-weight: bold; color: #DEABA0; background-color: #7B1F3F; padding: 5px 10px; line-height: 20px; display: inline-block; border-radius: 4px;">
      &#128161; Tip:
    </span>
    GEO allows you to filter for organism, tissue, assay type, and more in the left side menu and the top right per top organism.
  </div>
</div>
<br>

You should now be presented with the following... 

<img src="/assets/img/figure-13.png" alt="GEO series record" width="500"/>

GEO will also display:

- Information about the authors and publication under **<span style="color:green;">Contributors and Citation.</span>**
- **<span style="color:deeppink;">Analyze with GEO2R and Download RNA-seq counts</span>** tools.
- Information about each **<span style="color:purple;">sample and sequencing platform.</span>**
- **<span style="color:darkorange;">Bioproject ID.</span>** This ID is assigned once submitting raw sequencing data to the **Sequencing Read Archive (SRA).** This link can also take you to the SRA in addition to **<span style="color:red;">SRA Run Selector</span>** (a more direct link to raw data).
- Any **<span style="color:blue;">Supplementary files</span>** associated with the study.

<div style="position: relative; margin-top: 2rem;">
  <div style="background-color:#250D18; border-top: 37px solid #7B1F3F; padding: 10px 16px 10px; color: #DEABA0; position: relative;">
    <span style="position: absolute; top: -30px; left: 1px; font-weight: bold; color: #DEABA0; background-color: #7B1F3F; padding: 5px 10px; line-height: 20px; display: inline-block; border-radius: 4px;">
      &#x26A0; Important!:
    </span>
   Any uploaded supplementary data is down to the authors. For GEO, the authors must at least upload their normalised data in which their observations were made. You may also get lucky with uploaded differentially expressed gene (DEG) tables with raw sequencing read counts but this is not always guaranteed. For the sake of this course, we will generate our own DEGs.
  </div>
</div>
<br>

<details style="font-family: Arial, sans-serif; margin-bottom: 0; color: #250D18;">
  <summary style="color: #DEABA0; border: 2px solid #DEABA0; border-radius: 6px; padding: 0.5em; margin-bottom: 1.5em; cursor: pointer; font-weight: bold; background-color: #250D18;">
    More on GEO2R and download RNA-seq counts...
  </summary>

  <div style="border: 2px solid #DEABA0; border-radius: 6px; background-color: #250D18; padding: 1.5em; color: #DEABA0; margin-top: -2em;">
    Although we won't use GEO2R, it can help you perform very basic analysis. You can define your own groups/contrasts and perform differential gene expression (DGE) rather quickly. Be aware that this tool does not always work and is dependent on what data the authors have uploaded.
    <div class="mb-4"></div>
    <div style="text-align: center;">
      <img src="/assets/img/figure-14.png" alt="Analyze with GEO2R tool" width="90%" />
    </div>
    <div class="mb-4"></div>
    <div style="text-align: left;">
      The Download RNA-seq counts option will provide download links to all of the uploaded and NCBI-generated data, though beware as this may not contain the raw sequencing data.
    </div>
    <div style="text-align: center;">
      <div class="mb-4"></div>
      <img src="/assets/img/figure-15.png" alt="Download RNA-seq counts" width="1000"/>
      <div class="mb-4"></div>
    </div>
  </div>
</details>

<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

### 3.1.2. How to download the relevant data
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- **Through GEO, the easiest route to download is through <span style="color:red;">SRA Run Selector**</span> <strong><a href="https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA934824&o=acc_s%3Aa">here.</a></strong>
- For the sake of simplicity, **you will compare differential gene expression (DGE) between 2 cell lines.**
  - You will be downloading **6 normoxia** samples, 3 from HK-2 and 3 from 786-0 cell lines.

<div style="position: relative; margin-top: 2rem;">
  <div style="background-color:#250D18; border-top: 37px solid #7B1F3F; padding: 10px 16px 10px; color: #DEABA0; position: relative;">
    <span style="position: absolute; top: -30px; left: 1px; font-weight: bold; color: #DEABA0; background-color: #7B1F3F; padding: 5px 10px; line-height: 20px; display: inline-block; border-radius: 4px;">
      &#128221; Note:
    </span>
    A typical bulk RNA-seq experiment is a little more complex where you would compare DGE across multiple tissues/cell lines, different treatments/disease states, in addition to comparing respective controls.
  </div>
</div>

<br>

To do (see also video below): 

- [ ] Go to <strong><a href="https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA934824&o=acc_s%3Aa">SRA run selector</a></strong> and use the filter tool on the top left to select **8: Treatment**, then **normoxia**.
- [ ] **Select all 6 samples** in the bottom table.
- [ ] Click on the **sliding Selected tab** in the table above. This ensures you only get the data you select.
- [ ] Lastly, select **Accession list** option to the right of the selection tab. This downloads a text file called **SRR_Acc_List.txt**.

<video width="100%" controls loop="" muted="" autoplay="">
  <source src="/assets/img/SRA_selector.mp4" alt="How to find workshop dataset">
</video>
<br>

<details style="font-family: Arial, sans-serif; margin-bottom: 0; color: #7B1F3F;">
  <summary style="color: #DE7B21; border: 2px solid #DEABA0; border-radius: 6px; padding: 0.5em; margin-bottom: 1.5em; cursor: pointer; font-weight: bold; background-color: #7B1F3F;">
    Your resulting accessions...
  </summary>
<div style="border: 2px solid #DEABA0; border-radius: 6px; background-color: #7B1F3F; padding: 0em; color: #DE7B21; margin-top: -2em; /* pulls it closer to the summary */ transition: margin-bottom 1.5">
<pre><span>
SRR23454118
SRR23454119
SRR23454122
SRR23454124
SRR23454125
SRR23454126
</span></pre>
</div>
</details>

<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

   nf-core/fetchngs pipeline
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- To keep things as simple as possible for users, I have created a GitHub repository which contains all of the directory structures and relevant scripts to perform each task for us.
- This will hopefully make things easier for users who are not so confident with coding.
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

### 3.1.3. Log into HAWK
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- Lets first log onto HAWK. We will be doing all of our coding within VSCode.
- <b>If you do not have VSCode set-up, please let me know now so that we can get you up and running.</b>
- Note: those of you using Linux may have to use code to do this. I will include the code below each section.
<br>
- Firstly, make sure you are connected to the VPN. If you are on-campus, theres no need.
- In VSCode, click on the 'Remote Explorer' button, then click 'Connect to Host in New Window' button. This opens a new VSCode window with the remote host.
- You will be prompted (top box) to enter your password. Do this and hit enter.
- You are now connected to HAWK.

<img src="/assets/img/pre-3.png" alt="Connecting to HAWK" width="1000"/>


***Linux Users***
- In your shell, log into HAWK with:
```
ssh c.c1234567@hawklogin.cf.ac.uk
#Enter Password
```

<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

### 3.1.4. Fetch the project from GitHub
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- To get the project directory from GitHub, we simply copy and paste one line of code below.
- We will first want to **move into our scratch directory**.
- To do this, we first need to click on 'Explorer' and then 'Open Folder'.
- A drop-down box appears at the top of the screen with a filepath. We need to change this to `/scratch/c.c1234567` and then click 'OK'.
- You may be prompted 'Do you trust the authors this folder?'. Click 'Trust all authors' or 'Yes'.
- You will now see that the directory will be open on the left of the window. Note: Most of you won't have anything here as you have not used HAWK or scratch before. Mine is populated with various directories.

<img src="/assets/img/d1-fig1.png" alt="Connecting to HAWK and navigating to scratch directory" width="1000"/>

- We next want to fetch the project from GitHub. To do this, we need a terminal window open so we can paste the code into.
- Click on 'Terminal' at the top of your screen and select 'New Terminal'. This will open a terminal window at the bottom of your VSCode window.

<img src="/assets/img/d1-fig2.png" alt="Opening a terminal window in VSCode" width="1000"/>

- Now we can simply paste the following code into the terminal and hit enter:

```
git clone https://github.com/Gibbatron/rnaseq-course.git
```
- You will get a notification when things are done.
- To double check if the directory has been downloaded, we can use the `ls` command.
- We should also see that the directory is now in the 'Explorer' window on the left. This may not be the case. To update the window, we can simply click the 'Refresh Explorer' button.

<img src="/assets/img/d1-fig3.png" alt="Pulling the project from GitHub" width="1000"/>

- To see the contents of the downloaded directory, you can click on it to expand, or use the drop-down arrow next to it.

***Linux Users***
- use `cd` command to move to your scratch directory and then input the following command:

```
cd /scratch/c.c1234567
git clone https://github.com/Gibbatron/rnaseq-course.git
```
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

### 3.1.5. Set-up
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- We need to change permissions of the rnaseq-course directory so that any daughter files and directories will inherit the same permissions:
- In the terminal window paste the following:

```
chmod -R 777 rnaseq-course #777 gives read, write, and execute permissions for everyone

setfacl -d -m u::rwx,g::rwx,o::rwx rnaseq-course #this code gives same permissions to daughter files and directories that are made.
```

- We also need to change permissions of the scripts in the bin directory:

```
cd rnaseq-course/bin
chmod +x *.sh
```

- Note: When changing directories within the terminal window, **the explorer window does not update as you go**, neither does it update if you click the refresh button.
- Note: You can move in- and out of directories using the explorer window. Right-clicking on the directory and clicking on 'Open in Integrated Terminal' will open the directory in the terminal window. There will be another section added to the terminal window on the right with two options. One named 'bash', the other named 'bash *rnaseq-course*'. The 'bash' option is where you originally were (your scratch directory) and the 'bash *rnaseq-course*' option is in the rnaseq-course directory. Clicking between these moves you to the subsequent directory and you will notice the terminal window changes with this too.

<img src="/assets/img/d1-fig4.png" alt="changing directories within VSCode" width="1000"/>
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

  Required files
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- Now that we have the permissions etc completed, we can now edit the required files needed for the fetchngs pipe.
- You will see, by using the `ls` command, or clicking the drop down options on the directories that we have the following directory structure:

```
.
└── rnaseq-course/
    ├── bin/
    │   ├── differentialabundance.sh
    │   ├── download-ref-genome.sh
    │   ├── fetchngs.sh
    │   ├── generate-samplesheet.sh
    │   ├── make-table-diff-abundance.sh
    │   └── rnaseq.sh
    ├── resources/
    │   ├── conditions.csv
    │   ├── contrasts.csv
    │   ├── diff-abundance-params.yaml
    │   ├── example-conditions.csv
    │   ├── fetchngs-params.yaml
    │   ├── ids.csv
    │   ├── my.config
    │   └── rnaseq-params.yaml
    ├── input/
    │   ├── fastq/
    │   └── example_fastqs/
    └── output/
```

- **Note: We will be executing the pipeline from the parent directory (rnaseq-course)**
- **Note: In addition to above note, all file paths are in relation to the parent directory**

<br>

     resources/ids.csv

- Now that we have the sample IDs, we can go ahead and add them to the `ids.csv` file.
- This file is a comma-separated value (.csv) file that contains the list of the sample IDs that we just downloaded.

<details>
<summary><b>What is a comma-separated values (.csv) file?</b></summary>

<br>

- A .csv file is simple text file that stores tabular data such as text and numbers in a specific structured format.
- Each line of the file corresponds to one row in the table.
- Within each line, fields(columns) are separated by commas.
- For example, the .csv for the table below looks like:

<br>

 <table>
  <tr>
    <th>Column-1</th>
    <th>Column-2</th>
    <th>Column-3</th>
  </tr>
  <tr>
    <td>input1</td>
    <td>input2</td>
    <td>input3</td>
  </tr>
  <tr>
    <td>input4</td>
    <td>input5</td>
    <td>input6</td>
  </tr>
   <tr>
    <td>input7</td>
    <td>input8</td>
    <td>input9</td>
  </tr>
</table>

<br>

<pre><span style="color:crimson;">
Column-1,Column-2,Column-3
input1,input2,input3
input4,input5,input6
input7,input8,input9
</span></pre>

<br>

</details>

<br>

- To add our IDs to the .csv, we need to open `ids.csv`, delete the text already there, then paste our list straight in.
- To open the file in VSCode, we can click on it which will open the file in the main window, or we can right-click on the file and click 'Open With...'. This will open a drop-down menu at the top of the screen. Select 'CSV Editor'.
- This will load the file into the main window as a table rather than its native format - this will make it easier for you to edit and understand the structure of the file.
- I would reccomend just clicking on it to open though, as the file has only one column.

<img src="/assets/img/d1-fig5.png" alt="Opening a .csv file" width="1000"/>

- With the file open, delete the text in both rows and paste your IDs into the table.
- Now save the table with `File > Save`.
- To close the file, click on the 'X' next to the filename along the top of your window.

***Linux Users***
```
nano resources/ids.csv

# delete the text

# paste your list in:
SRR23454118
SRR23454119
SRR23454122
SRR23454124
SRR23454125
SRR23454126

# save the file
ctrl + x
y
enter
```

<br>

     resources/fetchngs-params.yaml

- This file contains all of the parameters needed for the pipeline to run.
- Instead of adding all of the options into the code when executing the pipeline, we can add them into this file. This keeps things tidier and easier to troubleshoot.

- **We need to change the email address in this file.**

- Open the file in VSCode by simply clicking on it, then change your email address.
- Then save the file by clicking `File > Save`. Close the file by clicking the 'X' next to the filename along the top of your window.

***Linux Users***
```
nano resources/fetchngs-params.yaml

# change your email:
input: resources/ids.csv
outdir: input
nf_core_pipeline: rnaseq
email: <b>your-email@cardiff.ac.uk</b>

# save the file
ctrl + x
y
enter
```

- input: Where the input ids.csv file is located. *Note: This is in relation to the parent directory.*
- outdir: Where to save the outputs to. *Note: This is in relation to the parent directory.*
- nf_core_pipeline: Formats the output data so that it conforms with the required inputs for the rnaseq pipeline that we will be using further down the line.

<br>

     resources/my.config

- This file contains all of the configuration code required for the pipeline to run correctly on HAWK.

- **We only need to change the email.**

- Open the file in VSCode by simply clicking on it, then change your email address and scw account.
- Then save the file by clicking `File > Save`. Close the file by clicking the 'X' next to the filename along the top of your window.

***Linux Users***
```
nano resources/my.config

# change your email:

params {
  config_profile_description = 'Super Computing Wales'
  config_profile_contact = 'your-email@cardif.ac.uk'
  config_profile_url = 'https://supercomputing.wales/'
  max_memory = 180.GB
  max_cpus = 20
  max_time = 72.h
}

singularity {
  enabled = true
  autoMounts = true
}

process {
  executor = 'slurm'
  queue = 'compute_amd'
}

executor {
  queueSize=10
}

process {
beforeScript = 'module load singularity-ce/3.11.4'
clusterOptions = '--account=scw2345'
}

# save the file
ctrl + x
y
enter
```
<br>
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

## 3.2. Executing the nf-core/prefetch pipeline
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">

- Now we have everything ready to execute the pipeline.
- To run the pipeline, we need to be in the **parent directory (rnaseq-course)** directory.
- We will be using tmux to execute the pipeline.

<details>
<summary><b>What is tmux?</b></summary>
- Tmux is a tool that we use to run multiple terminal sessions at once.
- We can use tmux to run our pipelines in the background, which leaves us to do other tasks in the meantime, or logout.
- If we were to run the pipeline without tmux, we would have to stay logged into HAWK until the pipeline has finished running.
- This can be problematic because 1) most pipelines can take a VERY long time to run, and 2) connection problems. If you are disconnected for any reason, the pipeline will cancel.
- Using tmux allows us to open a new terminal window, run the pipeline, and close the session so that it runs in the background.
- We can then log out of our HAWK session and log back in once we have been notified of the pipelines completion.

</details>


**Change to the cla1 node**

---

- <b>Jan'25 update:</b> we need to change to the amd compute node for the purpose of this course.
- To do this, enter the following in the terminal window:

```
ssh cla1
```

- You will be prompted continue. Type 'yes'.
- There will be text on the screen saying 'One-time SSH cleanup for Hawk gen 2...'.
- Wait for this to complete, you will then see '...done'.

---
**Launch a tmux session**

---

- From the **parent** directory (`rnaseq-course` - you will need to use the cd command to get there), run the following in the terminal window:

```
cd /scratch/c.c1234567/rnaseq-course/
module load tmux
tmux new -s fetchngs
```

- This loads the tmux module in HAWK and opens a new tmux session called fetchngs.

---
**Load Modules**

---

- With the tmux session opened, paste the following:

```
module load nextflow/24.10.4
module load singularity/singularity-ce/3.11.4
```

- This loads the required modules for the pipeline to run.

---
**Execute pipeline**

---

- Now we can go ahead and execute the pipeline.
- Usually, we would run the pipeline by typing out the command followed by the options etc etc.
- To make thinks simpler for everyone, and to avoid typos etc, I have added this command to a script (`bin/fetchngs.sh`).
- To execute the pipeline, all we need to do is run this script:

```
./bin/fetchngs.sh
```

- This will run the pipeline for us. Leave the pipeline run for a few minutes to ensure it is working, then we can close the session by doing the following:

<img src="/assets/img/d1-tmux.gif" alt="Opening a tmux session" width="1000"/>

<img src="/assets/img/d1-fig6.png" alt="Executing the fetchngs pipeline" width="1000"/>


```
Ctrl + b

then press d
```

- We will cover the outputs from this pipeline during the Day 2 session.

<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;"><hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">
    End of Day 1
<hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;"><hr style="height: 5px; background-color: #7B1F3F; border: none; width: 100%; border-radius: 10px;">
