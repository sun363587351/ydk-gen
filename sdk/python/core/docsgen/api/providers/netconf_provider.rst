NETCONF Service Provider
========================


.. py:class:: ydk.providers.NetconfServiceProvider(address, username, password, port=830, protocol='ssh', repo=None)

    Constructs an instance of the ``NetconfServiceProvider`` to connect to a server which **has** to support model download. Since the class is a Python wrapper for C++ ``NetconfServiceProvider`` class, which has clean up methods implemented in its destructor. The user does not need to worry about close NETCONF session.

    :param address: (``str``) IP address of the device supporting a netconf interface
    :param port: (``int``) The port to use, defaults to 830
    :param username: (``str``) Username to log in to the device
    :param password: (``str``) Password to log in to the device
    :param protocol: (``str``) Defaults to ``ssh``, currently support ``ssh``
    :param repo: User provided repository stores cached models
    :type repo: :py:class:`Repository<ydk.path.Repository>`

    .. py:method:: get_encoding()

        Returns the type of encoding supported by the service provider.

    .. py:method:: get_session()

        Returns the instance of the :py:class:`NetconfSession<ydk.path.NetconfSession>` used to connect to the netconf server

        :return: A :py:class:`NetconfSession<ydk.path.NetconfSession>` instance.
