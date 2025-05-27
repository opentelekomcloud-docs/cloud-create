.. _Service catalogs:

****************
Service catalogs
****************

In addition to infrastructure components (e.g., compute, network, storage), Cloud Create provides most frequently used services on the application layer that are ready-to-deploy (e.g., Prometheus for monitoring). This section shows how to use these services in the editor.

The service catalogs are modelled using TOSCA (i.e., a YAML file definition of the service). They are open-source and available at `the service catalogs of Open Telekom Cloud <https://github.com/opentelekomcloud-blueprints/tosca-service-catalogs>`_.

A part from the services provided by Cloud Create, users can also model their services using TOSCA, then import them in the designer and share with one another. For tutorials how to model the service using TOSCA, refer to :ref:`tosca define service catalog`.

.. toctree::
  :maxdepth: 1
  :hidden:

  openshift
  cce
  prometheus
  mysql
  rds
  apache
  nextcloud