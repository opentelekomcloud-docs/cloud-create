******************************
How to define an admin network
******************************

Users can set a private network as an admin network by checking the **admin_network** property in the network properties.

.. figure:: /_static/images/4-BastionHost-AdminNetwork.png
	:width: 800

	Figure 1. Define an admin network

.. note::

	**Auto-select admin network**: If users do not define an admin network, the designer will select a private network connecting to all computes as an admin network by default. Before the deployment starts, the designer also informs, which network is auto-selected as the admin network (figure below).

	.. figure:: /_static/images/4-BastionHost-AdminNetwork-auto.png
		:width: 800

		Figure 2. An information message that an admin network is auto selected before the deployment
