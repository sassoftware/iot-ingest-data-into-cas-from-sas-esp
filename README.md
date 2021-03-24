# Ingest data into CAS from ESP

How to use the SAS CAS _**loadStream**_ actionset to ingest data from an ESP running project into CAS.

## Overview

Two different techniques are available to ingest data from SAS ESP directly into a CAS environment:

- Data ingestion through the SAS ESP CAS adapter, or
- Data ingestion through the loadStream CAS actionSet

The attached document focuses on the CAS actionSet method and will demonstrate how CAS actions can be used programmatically to process any amounts of data in a very simple way.  This method applies to all versions of Viya, cloud and non-cloud based. The sample file referenced in the document is also provided to make it easy to replicate the process on your own environment.

## Prerequisites

Performing the steps documented in the paper requires familiarity with the following SAS products:

- SAS Studio
- SAS Event Stream Processing (ESP)
- SAS Event Stream Processing Studio (ESP Studio)
- For Viya 4 installations, a general knowledge of the Kubernetes architecture

With Viya 4, the code snippet provided in the paper and used to retrieve the IP address of the Kubernetes POD running the ESP project, requires the following:

- Execution of a command through a shell
- Availability of the **nslookup** OS command on the kubernetes cluster

Enabling the execution of commands through a shell is done through SAS Environment Manager, and it requires administrative privileges. Following is a list of the steps to perform:

- Log on to SAS Environment Manager;
- On the left side of the screen, click on "_**Contexts**_", then select "_**Compute contexts**_" from the "_**View**_" box;
- Select "_**SAS Studio compute context**_" from the list of available contexts for your Viya installation; 
- On the next screen, click on the pencil icon on the far side of the lne that reads "_**Basic Properties**_";
- The "_**Edit Compute Context**_" pop-up window appears. Scroll down to the "_**Attribute**_" section, then click on the plus (_**+**_) sign to add a new attribute;
- Type "_**allowXCMD**_" in the "_**Attribute:**_" field, and "_**true**_" in the "_**Value:**_" field. Select "_**Save**_" when done;
- Click on "_**Save**_" again to close the "_**Edit Compute Context**_" window;
- Start or restart a SAS Studio session, and run a code snippet similar to the following to verify that the previous steps have been completed successfully:

        %macro currdate;
               %let command = %bquote(")%str(date)%bquote(");

               filename shell pipe %unquote(&command);

               data _null_;
                    length buffer $150.;
                    infile shell dlm="¬";
                    input buffer;
                    call symputx('currdate', buffer, 'G'); 
               run;

               %put The current date and time is &currdate;
        %mend currdate;

        %currdate;

    The SAS log should display an output similar to the following:

        The current date and time is Thu Feb 11 21:47:28 UTC 2021

As for **nslookup**, if the utility is not available on your cluster, the **getent** command can be used in its place as shown in the paper.  

## Getting Started

With Viya 4, the sample data file (measures.csv) has to be pre-loaded on a folder that is local to the POD where the ESP project runs. As an example, the Viya 4 environment used to test the code had access to an ephemeral NFS volume, which is a NFS-mounted folder belonging to a server external to the Kubernetes cluster. Placing any file on that folder means that the POD running the ESP project has access to it. Please check with your Kubernetes administrator to see what storage options are available to you.  

## Contributing

 We welcome your contributions! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to submit contributions to this project. 

## License

 This project is licensed under the [Apache 2.0 License](LICENSE).

## Additional Resources

- [SAS Event Stream Processing](https://go.documentation.sas.com/?cdcId=espcdc&cdcVersion=v_004&docsetId=espwlcm&docsetTarget=home.htm&locale=en)
- [SAS Viya Administration](https://go.documentation.sas.com/?cdcId=sasadmincdc&cdcVersion=v_009&docsetId=sasadminwlcm&docsetTarget=home.htm&locale=en)
- [Blog posts](https://blogs.sas.com/)
- [SAS Communities](https://communities.sas.com/)
- [SAS Technical Support](https://support.sas.com)
