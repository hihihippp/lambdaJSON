lambdaJSON
===========
[![PyPi version](https://pypip.in/v/lambdaJSON/badge.png)](https://pypi.python.org/pypi/lambdaJSON)
[![PyPi downloads](https://pypip.in/d/lambdaJSON/badge.png)](https://pypi.python.org/pypi/lambdaJSON)

Serialize python standard types (function, tuple, class, memoryview, set, frozenset, exceptions, complex, range, bytes, bytearray, dict with number keys, byte keys or tuple keys, and etc) with json.
lambdaJSON lets you serialize python standard library objects with json.
Typical usage::

    #!/usr/bin/env python
    
    >>> import lambdaJSON
    >>> myComplexData = {True: (3-5j), (3+5j): b'json', (1, 2, 3): {b'lambda': [1, 2, 3, (3, 4, 5)]}}
    >>> mySerializedData = lambdaJSON.serialize(myComplexData)
    >>> myComplexData  == lambdaJSON.deserialize(mySerializedData)
    True
    
    >>> 

To pass args and kwargs to the encoder/decoder simply pass them to the serialize/deserialize function, example for json::

    >>> mySerializedData = lambdaJSON.serialize(myComplexData, sort_keys = True)
    >>> myComplexData  == lambdaJSON.deserialize(mySerializedData, object_hook = my_hook)

It can be done for ujson too. You can also serialize python functions::

    >>> import lambdaJSON
    >>> def f(): print('lambdaJSON Rocks!')
    
    >>> mySerializedFunction = lambdaJSON.serialize(f)
    >>> myNewFunction  = lambdaJSON.deserialize(mySerializedFunction)
    >>> myNewFunction()
    'lambdaJSON Rocks!'
    >>>

Changed int 0.2.4, for function deserialization you must pass a function which returns the list of globals for the function::

    >>> import lambdaJSON
    >>> y = 10
    >>> def f(x): return x*y
    
    >>> mySerializedFunction = lambdaJSON.serialize(f)
    >>> myNewFunction  = lambdaJSON.deserialize(mySerializedFunction, globs = (lambda: globals()))
    >>> myNewFunction(5)
    50
    >>> y = 3
    >>> myNewFunction(5)
    15
    >>>

If no globs passed to function, the globs will be just the __builtins__ module. Note that passing globals will pass the lambdaJSONs globals and it will not work, if you want to include all the globals from where the deserialization function is called, just use globs = (lambda: globals()), else implement your own function. You can do some nice hacks too::

    >>> z = 10
    >>> def g():
            global z
            z += 1
            return {'z':z}
    
    >>> def f(x,y): return x*y+z
    
    >>> mySerializedFunction = lambdaJSON.serialize(f)
    >>> myNewFunction  = lambdaJSON.deserialize(mySerializedFunction, globs = g)
    >>> myNewFunction(2,3)
    17
    >>> myNewFunction(2,3)
    18
    >>>

You can serialize Builtin Exceptions like this::

    >>> a = lambdaJSON.serialize(OSError('FILE NOT FOUND'))
    >>> b = lambdaJSON.deserialize(a)
    >>> raise b
    Traceback (most recent call last):
      File "<pyshell#3>", line 1, in <module>
        raise b
    OSError: FILE NOT FOUND
    >>>

introduced in version 0.2.15, you can now serialize basic classes and types. The support is basic, but I'm planning to develop the class serialization support in the next subversion. to deserialize a class, you must pass the globals function too, if you do not pass the globals, only __builtins__ will be passed to the class functions. this is an example to do it::

    >>> class test(object):
            def __init__(self):
                self.var = 'lambdaJSON'
    
    >>> serializedClass = lambdaJSON.serialize(test)
    >>> newClass = lambdaJSON.deserialize(serializedClass, globs = lambda: globals())
    >>> newClass().var
    'lambdaJSON'
    >>> 

To check version, simply use lambdaJSON.__version__, or if you want to know which json lib is in use, try lambdaJSON.__json__

After the support for all types are added, I'm planning to release a query friendly version of this library, that will be in version 0.3.0.

The json lib
============

LambdaJSON first tries to import ujson, if it fails it will import simplejson, and if that fails too, the json lib will be imported. you can check which json lib is in use with lambdaJSON.__json__ variable.

Currently Supported Types
=========================

This types are covered in this version:

1. Functions
2. Bytes and Bytearrays
3. Classes (basic support)
4. Builtin Exceptions
5. Tuples
6. Complex
7. Range
8. Set and Frozenset
9. Memoryview
10. Dicts (With Number, Tuple, String, Bool and Byte keys)
11. other json supported types

Changes from previous
=====================

Fixed a problem with function freezing.

Download
========

Download package from here: https://pypi.python.org/pypi/lambdaJSON

Project Info
============

Github project page: https://github.com/pooya-eghbali/lambdaJSON
PyPi: https://pypi.python.org/pypi/lambdaJSON
Mail me at: persian.writer [at] Gmail.com


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/pooya-eghbali/lambdajson/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
