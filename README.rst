===========
Stub Server
===========

.. image:: https://api.travis-ci.org/tarttelin/Python-Stub-Server.svg
        :target: https://travis-ci.org/tarttelin/Python-Stub-Server

.. image:: https://coveralls.io/repos/tarttelin/Python-Stub-Server/badge.svg?branch=master&service=github
        :target: https://coveralls.io/github/tarttelin/Python-Stub-Server?branch=master

Testing external web dependencies in a mock objects style. Written for Python
2.6, ported to Python 2.5 this library includes the tests at the bottom of
the stubserver.py file, which serve both as the TDD tests written while
creating this library, and as examples / documentation.  It supports any HTTP
method, i.e. GET, PUT, POST and DELETE.  It supports chunked encoding, but
currently we have no use cases for multipart support etc, so it doesn't do it.
An excerpt from the tests is below:

::

  class WebTest(TestCase):

      def setUp(self):
          self.server = StubServer(8998)
          self.server.run()

      def tearDown(self):
          self.server.stop()
          # implicitly calls verify on stop

      def test_put_with_capture(self):
          capture = {}
          self.server.expect(method="PUT", url="/address/\d+$", data_capture=capture)\
                     .and_return(reply_code=201)

          # do stuff here
          captured = eval(capture["body"])
          self.assertEquals("world", captured["hello"])

Though stubserver is at version 0.1, it is actively used in Python 2.5 to
Python 2.7 codebases, so it is fairly bug free.

There is also an FTPStubServer for your FTP testing needs, but that is NOT
bug free at the moment.  All assistance gratefully received.
