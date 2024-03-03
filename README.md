# Pig Lab Project

This Pig Lab project involves working through various examples and exercises using Pig scripts on the CSUEB Hadoop platform. Below are summaries of each part along with commands and steps for execution.

## Part 1: Max Temperature Example

### Summary
In this part, we work through the "max-temperature" example to find the maximum temperature recorded for each year.

### Commands and Steps
1. Load data and inspect:
    ```pig
    records = LOAD 'sample.txt' AS (year:chararray, temperature:int, quality:int);
    DUMP records;
    DESCRIBE records;
    ```

2. Group records by year:
    ```pig
    grouped_records = GROUP records BY year;
    DUMP grouped_records;
    DESCRIBE grouped_records;
    ```

3. Calculate max temperature:
    ```pig
    max_temp = FOREACH grouped_records GENERATE group, MAX(records.temperature);
    DUMP max_temp;
    ILLUSTRATE max_temp;
    ```
## Part 2: Macros Example

### Summary
This part demonstrates the usage of macros in Pig scripts to create reusable functionality.

### Commands and Steps
1. Define the macro:
    ```pig
    DEFINE max_by_group(X, group_key, max_field) RETURNS Y {
      A = GROUP $X by $group_key;
      $Y = FOREACH A GENERATE group, MAX($X.$max_field);
    };

    records = LOAD 'sample.txt' AS (year:chararray, temperature:int, quality:int);
    filtered_records = FILTER records BY temperature != 9999 AND quality IN (0, 1, 4, 5, 9);
    max_temp = max_by_group(filtered_records, year, temperature);
    DUMP max_temp;
    ```

## Part 3: Trim JAVA UDF Example

### Summary
This part demonstrates the creation and usage of a Trim UDF (User-Defined Function) implemented in Java.

### Commands and Steps
1. Load data and compile UDF:
    ```pig
    A = LOAD 'A' AS (fruit:chararray);
    DUMP A;
    ```
    ```bash
    sh javac -classpath /home/student9/hadoop-common-2.6.1.jar:/home/student9/hadoop-mapreduce-client-core-2.6.1.jar:/home/student9/commons-cli-2.0.jar:/home/student9/pig-0.11.0.jar -d . Trim.java
    ```

2. Package UDF into a JAR:
    ```bash
    sh jar -cvf pig-trim.jar com/hadoopbook/pig/Trim.class
    ```

3. Register JAR and use UDF:
    ```pig
    REGISTER pig-trim.jar;
    B = FOREACH A GENERATE com.hadoopbook.pig.Trim(fruit);
    DUMP B;
    ```

## Part 4: Strip Python UDF Example

### Summary
This part demonstrates the creation and usage of a Strip UDF implemented in Python.

### Commands and Steps
1. Register Python UDF and use it:
    ```pig
    REGISTER 'stripPig.py' USING streaming_python AS my_strip;
    C = FOREACH A GENERATE my_strip.strip_function(fruit);
    Dump C;
    ```

## Visuals

- Replace this text with screenshots for each part showing the execution and result of the Pig scripts.

## Note
- Ensure all paths and filenames are adjusted according to your Hadoop environment.
- Screenshots should be replaced with actual visuals for better understanding.
