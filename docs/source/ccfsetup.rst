.. _settingup:
Setting Up CCF
===============

This document is there to help us guide through each step on as to how to set up ccf. It will contain the following :   
 * :ref:`INFRA SETUP <infra>`
 * :ref:`DEPLOYMENT <deployment>`
 * :ref:`RUNBOOK <runbook>`
.. _infra:
INFRA SETUP
------------

For the INFRA SETUP of CCF process like ingestion and dw these following steps have to be followed :

   Set up AWS MWAA
   ----------------
   * Open the `Amazon MWAA console <https://console.aws.amazon.com/mwaa/home/`__.
   * Use the **AWS Region selector** to select your region.
   * Choose **Create environment**.
   
RDS setup
---------
   * Configurations:

    a. At the time of launch, an username and password is created which is used to access RDS. The access will be granted provided the credentials are correct and the AWS resource from which the access is requested has access to connect to RDS.
    b. Type - db.r5.xlarge
    c. Engine - Aurora MySQL 5.7.12
    d. Storage – 500 GB

Secret Manager
---------------
   * New secret in AWS Secret Manager having RDS credentials stored with         "username" and "password" keys.
SES
----
   * Emails should be verified in SES which will be used in configs for          sending notification mails.
EMR
----
   5.1. Following EMR configurations needed to configure CCF:
   a. Roles
   i. EMR role - Refer to https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-iam-role.html
   
   ii.EC2 instance profile - Refer to https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-launch-with-quick-options.html
   
   b. S3 folder - This option specifies the path to a folder in an Amazon S3 bucket where you want Amazon EMR to write log data.
   c. Security Groups
   i. ElasticMapReduce-Master-Private - For rules in this security group,
      see https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-man-sec-groups.html#emr-sg-elasticmapreduce-master-private
   ii.ElasticMapReduce-Slave-Private - For rules in this security group, https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-man-sec-groups.html#emr-sg-elasticmapreduce-slave-private
   iii.ElasticMapReduce-ServiceAccess - For rules in this security group, see https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-man-sec-groups.html#emr-sg-elasticmapreduce-sa-private
   1. Following EMR configurations needed to configure CCF:
   a. Roles
   i. EMR role - Refer to https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-iam-role.html
   ii.EC2 instance profile - Refer to https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-launch-with-quick-options.html
   b. S3 folder - This option specifies the path to a folder in an Amazon S3bucket where you want Amazon EMR to write log data.
   c. Security Groups
   i. ElasticMapReduce-Master-Private - For rules in this security group,see https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-man-sec-groups.html#emr-sg-elasticmapreduce-master-private
    ii.ElasticMapReduce-Slave-Private - For rules in this security group,
       https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-man-sec-groups.html#emr-sg-elasticmapreduce-slave-private
    iii.ElasticMapReduce-ServiceAccess - For rules in this security group,see https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-man-sec-groups.html#emr-sg-elasticmapreduce-sa-private

.. _deployment:
DEPLOYMENT
-----------

This section will help you in deploying RDS.

   RDS Schema Creation and Access
   -------------------------------
   A MySQL  engine based RDS was setup and launched in CC for storing our configurations and logs. 
   At the time of launch, a username and password is created which is used to access RDS. The access will be granted provided the credentials are correct and the AWS resource from which the access is requested has access to connect to RDS.

   Creation of Common Components Schema
   -------------------------------------
   After executing the Common Components Schema script the following tables will be created.

+------------------+
|ctl_dataset_master|
+------------------+
|ctl_dqm_master    |
+------------------+
|ctl_cluster_config|
+------------------+

.. _runbook:
RUNBOOK
--------

Now let's understand the execution of the profiler.

1. In the environment_params.json file, edit the following fields specifically:
 *dataset_list
 *layer_list
 *aws_aacount_id
2. Further, after uploading this JSON to the S3 Location and then providing it in Glue Job in Referenced Path (As mentioned in the Deployment Doc) Trigger the Job from Glue Console.

3. Further, after running the job, you may check the status and details of job in the Log Table – 
   *log_profiler_sumry*

