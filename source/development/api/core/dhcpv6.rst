Dhcpv6
~~~~~~

.. csv-table:: Resources (LeasesController.php)
   :header: "Method", "Module", "Controller", "Command", "Parameters"
   :widths: 4, 15, 15, 30, 40

    "``POST``","dhcpv6","leases","del_lease","$ip"
    "``GET``","dhcpv6","leases","search_lease",""
    "``GET``","dhcpv6","leases","search_prefix",""

.. csv-table:: Service (ServiceController.php)
   :header: "Method", "Module", "Controller", "Command", "Parameters"
   :widths: 4, 15, 15, 30, 40

    "``POST``","dhcpv6","service","reconfigure",""
    "``POST``","dhcpv6","service","restart",""
    "``POST``","dhcpv6","service","start",""
    "``GET``","dhcpv6","service","status",""
    "``POST``","dhcpv6","service","stop",""
