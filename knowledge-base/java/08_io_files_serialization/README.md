# Java I/O, Files, and Serialization

This section explains how Java works with files, paths, byte streams, character
streams, JSON data, and object serialization.

After finishing it, you should be able to read and write files safely, choose
between byte and character APIs, use try-with-resources, process larger files
without loading everything into memory, understand JSON as a common interchange
format, and explain why Java native serialization should be used carefully.

## Topics

- 01\. [Files and Paths](files_paths.md)
- 02\. [Byte Streams](byte_streams.md)
- 03\. [Readers and Writers](readers_writers.md)
- 04\. [Working With Larger Files](large_files.md)
- 05\. [JSON Basics](json_basics.md)
- 06\. [Java Serialization](java_serialization.md)
- 07\. [I/O Best Practices](io_best_practices.md)

## Suggested Learning Flow

Start with `Path` and `Files`, because they are the modern foundation for file
work. Then learn byte streams for binary data and readers/writers for text data.
After that, study larger-file processing, JSON basics, and Java serialization.
Finish with best practices, because I/O bugs often come from resource leaks,
encoding mistakes, path assumptions, and unsafe deserialization.

## Mini Goal

By the end of this section, write a small program that:

- builds file paths with `Path`;
- reads a UTF-8 text file;
- writes a report file;
- copies binary data with streams;
- processes a large file line by line;
- converts simple data to and from JSON using a library;
- avoids unsafe native Java deserialization.

## Interview Readiness

You should be able to answer:

- What is the difference between `Path` and `File`?
- When should you use byte streams instead of readers/writers?
- Why does character encoding matter?
- What problem does try-with-resources solve?
- How can you process a large file without loading it all into memory?
- Why is native Java deserialization risky?
- Why is JSON usually handled with a library instead of string concatenation?

