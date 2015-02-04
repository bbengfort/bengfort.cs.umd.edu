---
layout: post
title:  "Simple CSV Data Wrangling with Python"
tags: [csv, data wrangling, python, avro]
category: tutorials
comments:
share:
---

I wanted to write a quick post today about a task that most of us do routinely but often think very little about - loading CSV (comma-separated value) data into Python. This simple action has a variety of obstacles that need to be overcome due to the nature of serialization and data transfer. In fact, I'm routinely surprised how often I have to jump through hoops to deal with this type of data, when it feels like it should be as easy as JSON or other serialization formats.

The basic problem is this: CSVs have inherent schemas. In fact, most of the CSVs that I work with are dumps from a database. While the database can maintain schema information alongside the data, the scheme is lost when serializing to disk. Worse, if the dump is denormalized (a join of two tables), then the relationships are also lost, making it harder to extract entities. Although a header row can give us the names of the fields in the file, it won't give us the type, and there is nothing structural about the serialization format (like there is with JSON) that we can infer the type from.

That said, I love CSVs. CSVs are a compact data format - one row, one record. CSVs can be grown to massive sizes without cause for concern. I don't flinch when reading 4 GB CSV files with Python because they can be split into multiple files, read one row at a time for memory efficiency, and multiprocessed with seeks to speed up the job. This is in stark contrast to JSON or XML, which have to be read completely from end to end in order to get the full data (JSON has to be completely loaded into memory and with XML you have to use a streaming parser like SAX).

CSVs are the file format of choice for big data appliances like Hadoop for good reason. If you can get past encoding issues, extra dependencies, schema inference, and typing; CSVs are a great serialization format. In this post, I will provide you with a series of pro tips that I have discovered for using and wrangling CSV data.

Specifically, this post will cover the following:

- The basics of CSV processing with Python
- Avoiding Unicode issues in Python 2.7
- Using namedtuples or slots for memory efficiency
- Serializing data with a schema using Avro

Although we won't cover it in this post, using these techniques you have a great start towards multiprocessing to quickly dig through a CSV file from many different positions in it at once. Hopefully this intro has made CSV sound more exciting, and so let's dive in.

> [Read the complete article at District Data Labs](https://districtdatalabs.silvrback.com/simple-csv-data-wrangling-with-python)
