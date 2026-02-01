# bgen
Badge generator from svg template and csv data file

This is a rewrite of the original bgen project that was used as a learning project for me to learn rust.
The original code has been lost.

I write this document to keep track of what needs to be done and how the application worked before as 
there was a version of the application that was used in production by my family. 
It is now being past own to others that should also maintain the code but it no longer exists.

## Application design
Should have :
- 5 inputs
  - template-file, the svg file from which placeholder values should be replaced
  - data-file, the csv file containing the values to insert in the template file
  - mappings-file, a text file with the extension .bgen, it contains the header from the csv file followed by = (equals) 
    and then the value from the svg file.<br>
    (example : FullName=${fullname})
  - log-config-file : dedicated config file for log4rs lib, optional
  - config-file : configuration for the application itself [see app config section](#app-config)

- 1 output :
  - An file per line from the data input file where the file is identical to the template file with only the placeholders having been replaced.

The application should log progression after each generated file (can add more granularity).<br>
The application should log when it is finished.<br>
The application should log when an error occurs without falling the whole process, unless the same error happens 5 times in a row. Then something else is wrong<br>
Error handling should be in an enable / disable mode ⇒ Does / should this exist ? <br>

A skip mechanic configured with the "skip" block of the configuration file.

template.filename : supports the usage of the template placeholders to be replaced with the input data files content.<br>
template.extension : supports svg or pdf. svg ouput the file in the original format, pdf transforms the svg template file with the svg2pdf library (might need new config ?)<br>

skip_cluster.enable : should  the skip feature be enabled.<br>
skip_cluster.skip : should be an array / multiple ?<br>
skip_cluster.skip.enable : should the skip configuration be applied ?<br>
skip_cluster.skip.column_to_skip : name of the column in the data file to whom "value_to_skip_on" comes from.<br>
skip_cluster.skip.value_to_skip_on : the specific value the column should have for the whole line to be ignored.

<a name="app-config">The application configuration has :</a>
-
- log:
  - level: enum
  - error_handling: boolean

- template:
  - replacement_max_size: int
  - output_folder: string
  - filename: string
  - extension: enum

- data:
  - big_file_size: int
  - warn_big_file_size: boolean
  - abort_inconsistency: boolean
  - active_indicator_key: string

- skip_cluster:
  - enabled: boolean
  - skip:
    - enabled: false
    - column_to_skip: string
    - value_to_skip_on: string

See [Log4rs](https://crates.io/crates/log4rs) for logging configuration (log-config-file)

