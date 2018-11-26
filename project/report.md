# Automation on Drug Interactions Profiling :hand: fa18-423-06, fa18-423-03, fa18-423-02, fa18-423-05

| Yixing Hu, Kelvin Liuwie, Chandler Mick, Omkar Tamhankar
| yixihu@gmail.com, kliuwie@gmail.com, chmick@iu.edu, otamhank@iu.edu
| Indiana University
| hid: fa18-423-02, fa18-423-03, fa18-423-05, fa18-423-06
| github: [:cloud:](https://github.com/cloudmesh-community/fa18-423-03/blob/master/project-report/report.md)
| code: [:cloud:](https://github.com/cloudmesh-community/fa18-423-03/blob/master/project-code/README.md)  

**:mortar_board: Learning Objectives**

* Creating a program that can parse and query large data sets
* Exploring various "free" cloud servers to run our program
* Evaluating which cloud server provides the optimal working efficiency

## Introduction

For our project, we created a Python program that took raw data from the United
States Food and Drug Administration's Adverse Event Reporting System (FAERS) and
filtered out important components for a user to query the information simply.
The main issue that we noticed was that the important information that Food and
Drug Administration (FDA) was collecting was in large datasets that incredibly
difficult to breakdown analyze for the average users of the information. Users
of this information could be regulators, doctors, or even concerned consumers.
Therefore, we believe that our program simplifies the analysis of these big data
sets that are provided by the FDA and makes them fit for spreadsheets, such as
Excel, for more in-depth review of the drug data.

One of the most important issues in the pharmaceutical industry today is
determining the quality of a drug on the market [@www-PharmTech]. Currently
a pharmaceutical company needs only prove in human clinical trials that a drug
is safe when taken in isolation. However, humans are not basic research subjects
and are often taking a number of medications. As the state of the healthcare
system continues to fall victim to political cycles, it is always important to
make sure that information on the safety and effects of drugs on the market need
to be readily and easily available for review. We hope that our project
represents an example of what could be implemented in the marketplace to present
the information to those who need to be aware of the effects of the drugs that
they are interacting with.

## Dataset

The dataset is an Extensible Markup Language (XML) file from the FAERS on the
FDA's website. FAERS is a collection of reports from doctors, nurses, and
patients who have reported to the FDA side-effects of using certain commercial
drugs [@www-FAERS-Website]. This data is used by the FDA to formulate warnings
and regulations against drugs to warn consumers of the potential side effects
that come about from using pharmaceutical drugs.

The publicly available XML file is a 112 MB file with over 16 million rows of
data. Therefore, the file cannot be easily evaluated and assessed without
creating some sort of program to trim out the unnecessary data and store the
valuable data for running the query. The data that we analyzed with our program
covers the months of July 2018 to September 2018. However, a user would most
likely want to analyze data from previous quarters, which is possible with our
program.

The XML file contains 67 tags for each "adverse event report." An adverse event
report is a negative side effect to a drug from anything including migraines to
something as serious as death. Every tag includes information about the patient,
the side effect, the drug, treatment duration, etc. For this project, we decided
the most important tags that would want to be accessed would be the
reactionmeddrapt, medicinalproduct, and activesubstancename. These relate to the
reactions from the drug, the name of the drug, and the substances in the drug,
respectively.

## Implementation
![ProgramDesign](https://github.com/cloudmesh-community/fa18-423-03/blob/master/project-report/project_design.PNG)

### parser.py
Our parser.py file takes an XML file containing the initial FAERS raw data from
the website and converts it to a dataframe format to be stored in a CSV file.
This initial parse separates the three tags that we are looking to store for
our program (reactionmeddrapt, medicinalproduct, and activesubstancename). The
parser.py program flows into the project.py program where the dataframe format
allows the data to be easily manipulated.

### project.py
Our project.py file takes an XML file containing the initial FAERS raw data from
the website and converts the selected data to a CSV file. Using the parsing.py
function, the function takes our dataframe data and outputs the three tags that
we used in our project. This program takes the original, large dataset and forms
a smaller, compact dataset with the data that we deemed important.

### query.py
Our query.py file takes the compiled CSV file and allows a user to return
certain data based on inputted values. These inputted values can be certain
drugs, substances within the drugs, or the reactions of the drugs. This user
queried data can be used by the user to evaluate the specific results that they
are looking for. This CSV file can be transferred to a spreadsheet for simple 
data analysis.

## Amazon Web Services EC2 Usage

**Overview:**  
This document explains how to access AWS EC2 and how it was used to run the
program in the cloud server.

**Setup**  
Go to https://aws.amazon.com/. Click on "Sign In to the Console". If you do not
already have an account set up, create an account. In the "AWS services" search
bar, search "EC2" and click on the EC2 option. Under "Create Instance" click
"Launch Instance". Click the "Select" button next to "Amazon Linux 2 AMI (HVM),
SSD Volume Type". Click on "Review and Launch" then "Launch". In the drop-down
menu select "Create a new key pair", type in a key pair name, click "Download
Key Pair", select the check box below, and click "Launch Instance". In the green
box at the top of the screen click on Instance ID link, this will open the
instance.

**Accessing the instance**  
Open command prompt, if using Windows, or Terminal, if using Mac. Type in "ssh
-i ~/*key-pair-location*   ec2-user@*IPv4 Public IP*". This will connect you
into the EC2 instance.

**Uploading files into the instance**  
Download Cyberduck 2. Open the application. In the dropdown menu, select "SFTP
(SSH File Transfer Protocol)". In the Server field, enter the Public DNS. In the
Username field enter "ec2-user" [@fa18-423-03-cyberduck-ec2]. In the SSH Private
Key field select "Choose" from the dropdown menu and select your .pem key pair
file. Click on "Connect". Once connected through Cyberduck, you can simply drag
and drop files into the instance.

**Disclaimer**  
Our .xml files were greater than 1 GB in size. AWS EC2's free tier only offers
up to 1 GB memory therefore we would receive memory errors when running our
python files on the server.

## Microsoft Azure Cloud Shell Usage

**Overview:**
This document explains how to access Microsoft Azure and how Azure is used to
run programs in the cloud server.

**Setup**   
Go to https://portal.azure.com/ to register an Azure account. Once you get an
account, you can go to Azure Portal to access all the tools you need. The search
bar at the very top could help you access any resource you need in Azure.

**Create a Python web app through Azure app services**  
First, clone the folder containing code files and data files from Github with
Bash/Terminal in your local machine. Before further operation, run the Python
files locally to check if the program runs correctly. Then, go back to the Azure
portal which was accessed previously. There, you will open the Cloud Shell for
the rest of your operations. The very first step in the Cloud Shell is to create
a deployment user, if you do not already have one. The command should read:

  > "az webapp deployment user set --user-name <username> --password <password>"

where <username> and <password> are replaced with the username and password you
are planning to use. After this command is ran successfully, there should be a
JSON output with your password shown as null. The username and the password
should be recorded for future use. After that, you have to create a resource
group to make it easier to manage, by using the command:

  > "az group create --name myResourceGroup --location "East US""

For this command, "East US" could be replaced by other regions where Azure is
available, but for the connection stability, it would be best to use the closest
region. Then, an Azure App Service plan should be created. The command should
read:

  > "az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux"

When those steps are done, it is the time to create the web app. Still working
on the Azure Cloud Shell, type the following command lines:

  > "az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PYTHON|3.7" --deployment-local-git"

These command lines should return an output starting with:

  > "Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'"

This URL of git should be kept as you will need it later for connection.

After those steps, go back to local bash for the following operations. The first
step is to add an Azure remote to the local Git repository by using the command:

  > "git remote add azure <deploymentLocalGitUrl-from-create-step>"

And then use:

  > "git push azure master"

to push to Azure form Git repository. This command should take a bit longer to
process. Once the processing is done, you can go back to the Azure Portal to
access the App Services to find the one being created. Click on that service and
go to the side bar to click on the SSH under Development Tools. Then, you
can access all the files you have pushed onto the cloud server.

## Results



## Conclusion



## Work Breakdown
* Yixing Hu: Microsoft Azure Server Testing
* Kelvin Liuwie: Parsing and Project Code Writer
* Chandler Mick: Report Writer, IBM VirtualBox Testing
* Omkar Tamhankar: AWS Server Testing




