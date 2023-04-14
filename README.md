# SQLite-Python-Tutorial
![SQLite-Python](https://user-images.githubusercontent.com/117528133/231989116-846ffa59-424a-4dec-a54f-f25986e036e7.jpg)

This section shows you step by step how to work with the SQLite database using Python programming language.

Python provides two popular interfaces for working with the SQLite database library: PySQLite and APSW. Each interface targets a set of different needs.

PySQLite
The PySQLite provides a standardized Python DBI API 2.0 compliant interface to the SQLite database. If your application needs to support not only the SQLite database but also other databases such as MySQL, PostgreSQL, and Oracle, the PySQLite is a good choice.

PySQLite is a part of the Python standard library since Python version 2.5

APSW
If your application needs to support only the SQLite database, you should use the APSW module, which is known as Another Python SQLite Wrapper.

The APSW provides the thinnest layer over the SQLite database library. The APSW is designed to mimic the native SQLite C, therefore, whatever you can do in SQLite C API, you can do it also from Python.

Besides covering the SQLite library, the APSW provides many low-level features including the ability to create user-defined aggregate, function, and collations from Python. It even allows you to write a virtual table implementation using Python.
