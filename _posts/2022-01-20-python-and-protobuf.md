---
layout: post
title:  Protobuf/gRPC and Python
author: Zhuyi Xue
tags: python protobuf
---

<style type="text/css">
  .gist-data {max-height: 500px;}
</style>

### Protobuf and Python

Proto files (`.proto`) can be compiled to Python code (`_pb2.py`) using `protoc`
from [protobuf repo](https://github.com/protocolbuffers/protobuf). E.g.

```
protoc --proto_path=proto --python_out=. foo.proto
```

For a simple proto file like

{% highlight Protobuf %}
syntax = "proto3";

message Foo {
  string input = 1;
}
{% endhighlight %}


The generated Python code looks like

{% gist c4e5b433cecb3fd249342756984d91bd %}

Note, I reformatted the generated code with
[black](https://github.com/psf/black) for better readability.

The generated code is relatively lightweight because the main mechanism for
building the corresponding Python class (`class Foo`) is still implemented in
the [`python` directory of the protobuf
repo](https://github.com/protocolbuffers/protobuf/tree/master/python), using a
Python
[metaclass](https://docs.python.org/3/reference/datamodel.html#customizing-class-creation),
instead of in the generated code. The implementation of the key metaclass
`GeneratedProtocolMessageType` can be found in
[`python/google/protobuf/internal/python_message.py`](https://github.com/protocolbuffers/protobuf/blob/01e84b129361913e5613464c857734fcfe095367/python/google/protobuf/internal/python_message.py#L77).

Note, the protobuf repo implements all of the proto compiler (`protoc`), the
protobuf Python library as well as runtime libraries for about nine other
languages.

The python part of protobuf repo is published on PyPi also by the name
[protobuf](https://pypi.org/project/protobuf/#history).

### gRPC and Python

Proto files (`.proto`) with `service` definitions can be compiled to Python modules
(`_pb2.py` and `_pb2_grpc.py`) using
[`grpcio-tools` from PyPi](https://pypi.org/project/grpcio-tools/#history).

E.g.

```
python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. my_service.proto
```

Note, the implementation `grpcio-tools` is in the [`tools/distrib/python`
directory] of the [grpc
repo](https://github.com/grpc/grpc/tree/master/tools/distrib/python/grpcio_tools).


For a simple proto file

{% highlight Protobuf %}
syntax = "proto3";

message Foo {
    string input = 1;
}

message Bar {
    uint32 output = 1;
}

service MyService{
    rpc GetBar(Foo) returns (Bar);
}
{% endhighlight %}

The generated `_pb2.py` file is like

{% gist 5a778790500cba53a282b7c7ff9974c3 %}

Note it contains two `_descriptor.Descriptor` instances (`_FOO` and `_BAR`) and
one `_descriptor.ServiceDescriptor` instance (`_MYSERVICE`), and two Python
classes (`Foo` and `Bar`) created via metaclass `GeneratedProtocolMessageType`,
similar to what's described in the `Protobuf and Python` section.

The generated `_pb2_grpc.py` file is like

{% gist e6b7de89923c04a02b6b26ea25ea6d9a %}

Note it implements three classes:

* `MyServiceStub`, to be used by a gRPC client for sending request to the gRPC server.
* `MyServiceServicer`, to be subclassed in the gRPC server code for implementing
  the real logic of `GetBar`
* `MyService`: an experimental API.

and a function `add_MyServiceServicer_to_server`, which is used to add the
subclass of `MyServiceServicer` to an
[`grpc._server._Server`](https://github.com/grpc/grpc/blob/master/src/python/grpcio/grpc/_server.py#L952)
instance (returned by
[`grpc.server`](https://github.com/grpc/grpc/blob/88ff7f0d3f09cfd577740c88c119f69942e50d08/src/python/grpcio/grpc/__init__.py#L2034-L2072)
function).

The generated code imports `grpc` module, which is implemented in the
[`src/python/grpcio`
directory](https://github.com/grpc/grpc/tree/master/src/python/grpcio) of the
grpc repo.

### References

* The post
  [https://realpython.com/python-metaclasses/](https://realpython.com/python-metaclasses/)
  has an informative intro to Python metaclasses. That a class is an instance of a metaclass is like a typical object being an instance of a class.
* Protobuf Python tutorial:
  [https://developers.google.com/protocol-buffers/docs/pythontutorial](https://developers.google.com/protocol-buffers/docs/pythontutorial)
* gRPC Python tutorial:
  [https://grpc.io/docs/languages/python/basics/](https://grpc.io/docs/languages/python/basics/)
* Demo code is available on https://github.com/zyxue/proto_grpc_python_demos.
