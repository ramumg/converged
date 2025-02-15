# Extract a smaller graph from the main graph

## Introduction

This lab is optional. We should be doing this lab only if we have uploaded the medical records in Lab 3, Task 3, and you want to break the input and analyze. In this lab, we consider the few clusters identified when you run Lab 6 using community detection. We further create the tables with nodes and edges with this newly extracted data for further analysis to identify clusters using the Infomap algorithm. The use of a small graph helps us understand the detected communities better.

Estimated Time: 30 minutes

### Objectives

- Extract the smaller graph from the main graph as it will be easier to understand the community analysis.

### Prerequisites

This lab assumes you have the following:

- All previous labs expect Lab 5 should be completed successfully.

## Task 1: Choose the clusters from the result of Infomap from the main graph

These are a few of the clusters formed after running community detection on the main graph we created in Lab 6. We have chosen the three clusters for breaking the data into smaller ones. The three clusters are related to Patient Details, Medications, and Orders. We take the nodes of these clusters and the connected edges to these nodes and dump the data to newly created tables. The patient Details cluster has 28 nodes, the Medications cluster has 18 nodes, and the Orders cluster has 17 nodes.

1. Below are the tables from the Patient Details cluster. Look into the below tables in the output. These tables will have the same community id in the output of Infomap. These are comma-separated values of tables from the Patient Details cluster. No action is required here.

    ```text
    ESO_TRIGGER,PRSNL_ORG_RELTN,PROBLEM_COMMENT,MATCH_TAG_PARMS,CODE_CDF_EXT,CREDENTIAL,FILL_CYCLE_BATCH,DEPT_ORD_STAT_SECURITY,WORKING_VIEW_FREQ_INTERVAL,PRSNL_RELTN,PE_STATUS_REASON,PERSON_CODE_VALUE_R,CMT_CONCEPT,SCH_LOCK,PROC_PRSNL_RELTN,TRACKING_EVENT_HISTORY,CODE_VALUE_SET,PERSON_PRSNL_RELTN,DOSE_CALCULATOR_UOM,CLINICAL_SERVICE_RELTN,ENCNTR_PRSNL_RELTN,PREDEFINED_PREFS,PSN_PPR_RELTN,CODE_VALUE_EXTENSION,PRSNL,CODE_VALUE,PRSNL_RELTN_ACTIVITY,CMT_CONCEPT_EXTENSION
   ```

2. Below are the tables from the Medications cluster. Look into the below tables in the output. These tables will have the same community id in the output of Infomap. These are comma-separated values of tables from the Medications cluster. No action is required here.

    ```text
      ORDER_CATALOG_ITEM_R,WARNING_LABEL_XREF,PACKAGE_TYPE,MED_IDENTIFIER,MED_PACKAGE_TYPE,MED_PRODUCT,MANUFACTURER_ITEM,WARNING_LABEL,MED_FLEX_OBJECT_IDX,MED_DEF_FLEX,ITEM_LOCATION_COST,QUANTITY_ON_HAND,MED_DISPENSE,MEDICATION_DEFINITION,MED_COST_HX,MED_INGRED_SET,MED_OE_DEFAULTS,RX_CURVE
   ```

3. Below are the tables from the Orders cluster. Look into the below tables in the output. These tables will have the same community id in the output of Infomap. These are comma-separated values of tables from the Orders cluster. No action is required here.

    ```text
      RENEW_NOTIFICATION_PERIOD,ORDER_REVIEW,BILL_ONLY_PROC_RELTN,FILM_USAGE,ORDER_CATALOG_SYNONYM,ORDERS,ORDER_INGREDIENT,ORDER_CATALOG,SCH_APPT_ORD,ORDER_ACTION,ORDER_IV_INFO,RAD_PROCEDURE_ASSOC,ORDER_NOTIFICATION,RAD_PRIOR_PREFS,RAD_FOLLOW_UP_RECALL,ACTIVITY_DATA_RELTN,ECO_QUEUE
   ```

4. Consider the tables from the above clusters related to Patients, Medications, and Orders. Merging the tables from these clusters by enclosing all tables in a single quote with a comma separator forms the IN QUERY format in SQL. We call the below text format 'TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT' as an alias, which we will replace text in queries used in Task 2.

    ```text
    <copy>
    'ESO_TRIGGER','PRSNL_ORG_RELTN','PROBLEM_COMMENT','MATCH_TAG_PARMS','CODE_CDF_EXT','CREDENTIAL','FILL_CYCLE_BATCH','DEPT_ORD_STAT_SECURITY','WORKING_VIEW_FREQ_INTERVAL','PRSNL_RELTN','PE_STATUS_REASON','PERSON_CODE_VALUE_R','CMT_CONCEPT','SCH_LOCK','PROC_PRSNL_RELTN','TRACKING_EVENT_HISTORY','CODE_VALUE_SET','PERSON_PRSNL_RELTN','DOSE_CALCULATOR_UOM','CLINICAL_SERVICE_RELTN','ENCNTR_PRSNL_RELTN','PREDEFINED_PREFS','PSN_PPR_RELTN','CODE_VALUE_EXTENSION','PRSNL','CODE_VALUE','PRSNL_RELTN_ACTIVITY','CMT_CONCEPT_EXTENSION','ORDER_CATALOG_ITEM_R','WARNING_LABEL_XREF','PACKAGE_TYPE','MED_IDENTIFIER','MED_PACKAGE_TYPE','MED_PRODUCT','MANUFACTURER_ITEM','WARNING_LABEL','MED_FLEX_OBJECT_IDX','MED_DEF_FLEX','ITEM_LOCATION_COST','QUANTITY_ON_HAND','MED_DISPENSE','MEDICATION_DEFINITION','MED_COST_HX','MED_INGRED_SET','MED_OE_DEFAULTS','RX_CURVE','RENEW_NOTIFICATION_PERIOD','ORDER_REVIEW','BILL_ONLY_PROC_RELTN','FILM_USAGE','ORDER_CATALOG_SYNONYM','ORDERS','ORDER_INGREDIENT','ORDER_CATALOG','SCH_APPT_ORD','ORDER_ACTION','ORDER_IV_INFO','RAD_PROCEDURE_ASSOC','ORDER_NOTIFICATION','RAD_PRIOR_PREFS','RAD_FOLLOW_UP_RECALL','ACTIVITY_DATA_RELTN','ECO_QUEUE'
    </copy>
    ```

## Task 2: Execute the SQL queries to get all the connected nodes for the selected clusters

Go to SQL developer and execute the below queries using the `TKDRADATA` user.

1. Replace #{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT} with values copied from Task 1, Step 4, and run the below SQL to fetch from columns TABLE1 and TABLE2 names from the EDGES table and combine the output and remove the duplicates.

    ```text
    <copy>
    SELECT DISTINCT TABLE1 FROM EDGES WHERE TABLE1 IN (#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT}) OR TABLE2 IN (#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT});
    </copy>
    ```

    ```text
    <copy>
    SELECT DISTINCT TABLE2 FROM EDGES WHERE TABLE1 IN (#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT})OR TABLE2 IN (#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT});
    </copy>
    ```

2. By combining the results of the above two queries, you will see all the connected tables for the nodes of the above three clusters(Patient, Medications, Orders). Enclosing all resultant tables in a single quote with a comma separator forms the IN QUERY format in SQL. We call the below text format 'TABLES-INCLUDING-CONNECTED-NODES' as an alias.
  
    ```text
    <copy> 
    'WARNING_LABEL_XREF','BILL_ITEM','SCH_APPT','ACCESSION','TRACKING_PRV_RELN','ORDER_CONTAINER_R','PE_STATUS_REASON','ORGANIZATION','PROC_PRSNL_RELTN','SIGN_LINE_FORMAT_DETAIL','SHARED_LIST_GTTD','ORG_ALIAS_POOL_RELTN','ORDER_CATALOG','PERSON_NAME','TRACK_EVENT','ENCNTR_INFO','ORDER_ENTRY_FIELDS','TRACKING_EVENT_HISTORY','CHARGE_EVENT_ACT','MED_COST_HX','ORDER_ACTION','SCH_EVENT_ALIAS','ECO_QUEUE','MLTM_NDC_MAIN_DRUG_CODE','PREDEFINED_PREFS','UCMR_CASE_TYPE','LOCATION','FILL_PRINT_ORD_HX','SCH_EVENT_ATTACH','ESI_LOG','MANUFACTURER_ITEM','ACCESSION_ORDER_R','BLOB_REFERENCE','V500_EVENT_SET_CODE','ORDER_DISPENSE','HM_EXPECT_MOD','ALT_SEL_CAT','SIGN_LINE_FORMAT','SCH_EVENT_PATIENT','CODE_VALUE_GROUP','ORDER_SENTENCE','SCD_TERM_DATA','ITEM_DEFINITION','CODE_VALUE_EVENT_R','RAD_INT_CASE','PRSNL','CODE_VALUE_EXTENSION','PERSON_INFO','ACTIVITY_DATA_RELTN','ENCNTR_PRSNL_RELTN','PROBLEM','LOGICAL_DOMAIN','TRACK_REFERENCE','PRSNL_GROUP_RELTN','DCP_FORMS_ACTIVITY_COMP','TASK_ACTIVITY','RAD_EXAM','PAT_ED_FAVORITES','MED_PRODUCT','ORDERS','REACTION','ORDER_INGREDIENT','ALT_SEL_LIST','DCP_SHIFT_ASSIGNMENT','IMAGE_CLASS','SCH_LOCK','SCH_EVENT','TRACK_GROUP','ACT_PW_COMP','PM_TRANSACTION','DCP_CARE_TEAM_PRSNL','PERSON_ALIAS','TRACKING_EVT_CMT','PW_CAT_SYNONYM','SCH_APPT_ORD','CODE_VALUE_SET','PPR_CONSENT_STATUS','PAT_ED_DOC_ACTIVITY','MED_OE_DEFAULTS','REGIMEN_CATALOG','STICKY_NOTE','SCH_LOCATION','CE_MED_RESULT','ORDER_TASK','ORDER_CATALOG_SYNONYM','PERSON_COMBINE_DET','ORDER_DETAIL','NURSE_UNIT','RAD_TECH_CMT_DATA','CODE_CDF_EXT','RAD_PROTOCOL_DEFINITION','ORG_BARCODE_FORMAT','COLLECTION_PRIORITY','RAD_PROTOCOL_ACT','NETTING','SCH_ENTRY','CHARGE_EVENT','HM_EXPECT_MOD_HIST','ORG_BARCODE_ORG','V500_EVENT_SET_EXPLODE','ORDER_PRODUCT_DOSE','SCH_SIMPLE_ASSOC','PRSNL_ALIAS','MED_DISPENSE','DISPENSE_CATEGORY','RAD_FOL_UP_FIELD','PATHWAY','IM_STUDY','CODE_VALUE_ALIAS','DIAGNOSIS','RX_CURVE','CMT_CONCEPT_EXTENSION','RAD_RES_INFO','ALLERGY','ALLERGY_COMMENT','IM_STUDY_PARENT_R','PRSNL_ORG_RELTN','PCS_DEMOGRAPHIC_FIELD','DEVICE','OE_FORMAT_FIELDS','MED_PACKAGE_TYPE','RAD_RPT_LOCK','PRSNL_RELTN','CHARGE_EVENT_ACT_PRSNL','PPR_CONSENT_POLICY','MED_DEF_FLEX','RAD_REPORT','RAD_FOLLOW_UP_RECALL','UCM_CASE_STEP','MAMMO_FOLLOW_UP','TRACKABLE_OBJECT','MAMMO_STUDY','CODE_VALUE','TRACKING_PRSNL','SCR_PATTERN','ENCNTR_LOC_HIST','PHARMACY_NOTES','SCH_EVENT_ACTION','ENCNTR_ALIAS','LONG_BLOB','PW_CAT_FLEX','PERSON_PRSNL_ACTIVITY','FILM_USAGE','DEPT_ORD_STAT_SECURITY','MED_FLEX_OBJECT_IDX','TASK_ACTIVITY_ASSIGNMENT','PROBLEM_PRSNL_R','CE_EVENT_PRSNL','PERSON_PRSNL_RELTN','MEDICATION_DEFINITION','RAD_REPORT_PRSNL','CMT_CONCEPT','ORDER_INGREDIENT_DOSE','LONG_TEXT','TRACKING_CHECKIN','TASK_DISCRETE_R','ORDER_RADIOLOGY','QUANTITY_ON_HAND','PM_WAIT_LIST_STATUS','RAD_FOLLOW_UP_CONTROL','EXAM_DATA','ICLASS_PERSON_RELTN','MEDIA_EXAM','TRACKING_EVENT','ESO_TRIGGER','MRU_LOOKUP_ED_DOC','PROBLEM_COMMENT','TEMPLATE_NONFORMULARY','PROCEDURE_SPECIMEN_TYPE','SCH_APPT_OPTION','PERSON','PRICE_SCHED','PERSON_PERSON_RELTN','PAT_ED_SHORTCUT','MED_IDENTIFIER','RAD_PROCEDURE_GROUP','ORDER_COMMENT','FILL_CYCLE_BATCH','WORKING_VIEW_FREQ_INTERVAL','PERSON_COMBINE','LOCATION_GROUP','ORDER_IV_INFO','PERSON_CODE_VALUE_R','DCP_FORMS_ACTIVITY_PRSNL','ORDER_THERAP_SBSTTN','OUTPUT_DEST','CONTAINER','ORDER_NOTIFICATION','TASK_RELTN','SIGN_LINE_DTA_R','SERVICE_DIRECTORY','REGIMEN_CAT_SYNONYM','PSN_PPR_RELTN','MED_INGRED_SET','PAT_ED_RELTN','SCD_STORY','ORDER_CATALOG_ITEM_R','RENEW_NOTIFICATION_PERIOD','CE_EVENT_ACTION','PHONE','TRACKING_LOCATOR','CS_COMPONENT','PACKAGE_TYPE','MATCH_TAG_PARMS','ORDER_REVIEW','CREDENTIAL','BILL_ONLY_PROC_RELTN','PCT_CARE_TEAM','RAD_FILM_ADJUST','UCM_CASE','FILM_EXAM','CLINICAL_EVENT','ORDER_TASK_RESPONSE','PFT_ENCNTR','WARNING_LABEL','ORD_RQSTN_ORD_R','PENDING_COLLECTION','PATHWAY_CATALOG','DCP_FORMS_ACTIVITY','ORDER_PRODUCT','NOMENCLATURE','ORDER_SUPPLY_REVIEW','TRACKING_ITEM','RAD_INT_CASE_R','RAD_PRIOR_PREFS','PRIV_LOC_RELTN','ITEM_LOCATION_COST','ORDER_LABORATORY','PRSNL_GROUP','DOSE_CALCULATOR_UOM','RAD_PROCEDURE_ASSOC','CLINICAL_SERVICE_RELTN','ENCOUNTER','PRSNL_RELTN_ACTIVITY','ORDER_TASK_XREF','EXPEDITE_COPY','SYNONYM_ITEM_R','ORG_TYPE_RELTN','PRINTER','ORG_ORG_RELTN','OCS_FACILITY_R','IMAGE_CLASS_TYPE','DEVICE_XREF','DCP_ENTITY_RELTN','RAD_INIT_READ','ROUTE_FORM_R','RAD_REPORT_DETAIL','PROXY_GROUP','TRACKING_PRSNL_REF','OUTBOUND_FIELD_PROCESSING','CODE_SET_EXTENSION','DRC_PREMISE','PRIVILEGE','PROC_CLASSIFICATION','TRACKING_EVENT_ORD','SHARED_VALUE_GTTD','BILL_ITEM_MODIFIER','DISCRETE_TASK_ASSAY','RES_SIGN_ACT_SUBTYPE'
    </copy>
    ```

## Task 3: Create new tables for the extracted data

1. Create new table `NODES_259_NEW` and replace the `#{TABLES-INCLUDING-CONNECTED-NODES}` copied in previous step(Task 2, Step 2). Populate the records of the connected nodes by running the following SQL.

    ```text
    <copy>
   CREATE TABLE NODES_259_NEW AS SELECT * FROM NODES WHERE TABLE_NAME IN (#{TABLES-INCLUDING-CONNECTED-NODES});
    </copy>
    ```

2. Create a new table, `EDGES_259_NEW`, and replace the `#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT}` by copying values from Task 1, Step 4. Populate the records of the connected edges by running the following SQL.

    ```text
    <copy>
    CREATE TABLE EDGES_259_NEW AS 
    SELECT * FROM EDGES WHERE 
    TABLE1 IN (#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT}) OR 
    TABLE2 IN (#{TABLES-FROM-SELECTED-CLUSTERS-IN-SQL-FORMAT});
    </copy>
    ```

3. Adding primary and foreign key constraints for the newly created tables `NODES_259_NEW` and `EDGES_259_NEW` by running the following SQL.

    ```text
    <copy>
    ALTER TABLE NODES_259_NEW ADD PRIMARY KEY (TABLE_NAME);
    ALTER TABLE EDGES_259_NEW ADD PRIMARY KEY (TABLE_MAP_ID);
    ALTER TABLE EDGES_259_NEW MODIFY TABLE1 REFERENCES NODES_259_NEW (TABLE_NAME);
    ALTER TABLE EDGES_259_NEW MODIFY TABLE2 REFERENCES NODES_259_NEW (TABLE_NAME);
    commit;
    </copy>
    ```

    Once this has been completed, you are ready to **proceed to the next lab**.

4. Create the graph using node table `NODES_259_NEW` and edge table `EDGES_259_NEW`. Follow the steps from Lab 4, Task 2, to create a graph. The only difference here is to select the newly created tables `NODES_259_NEW` and `EDGES_259_NEW` for graph creation.

## Task 4: Alternative approach for creation of smaller graphs

This task is not required if you have followed through with Task 1 to Task 3.

1. Download the below files from the cloud shell and directly upload the same CSV files into DB as in Lab 3 -> Task 3.

- `microservices-data-refactoring/livelabs/resources/NODES_259_NEW.csv` - Where we have table names.
- `microservices-data-refactoring/livelabs/resources/EDGES_259_NEW.csv` - Where we have source(TABLE1) and destination(TABLE2) columns with the edge.

2. Adding primary and foreign key constraints for the newly created tables `NODES_259_NEW` and `EDGES_259_NEW` by running the following SQL.

    ```text
    <copy>
    ALTER TABLE NODES_259_NEW ADD PRIMARY KEY (TABLE_NAME);
    ALTER TABLE EDGES_259_NEW ADD PRIMARY KEY (TABLE_MAP_ID);
    ALTER TABLE EDGES_259_NEW MODIFY TABLE1 REFERENCES NODES_259_NEW (TABLE_NAME);
    ALTER TABLE EDGES_259_NEW MODIFY TABLE2 REFERENCES NODES_259_NEW (TABLE_NAME);
    commit;
    </copy>
    ```

3. Create the graph using node table `NODES_259_NEW` and edge table `EDGES_259_NEW`. Follow the steps from Lab 4, Task 2, to create a graph. The only difference here is to select the tables `NODES_259_NEW` and `EDGES_259_NEW`.

4. Run the Infomap algorithm on the new graph. Execute the instructions from Lab 6 to run community detection, and analyze the results.

Once this has been completed, you are done with the lab.

## Acknowledgements

- **Author** - Praveen Hiremath, Developer Advocate
- **Contributors** -  Praveen Hiremath, Developer Advocate
- **Last Updated By/Date** - Praveen Hiremath, Developer Advocate, October 2022
