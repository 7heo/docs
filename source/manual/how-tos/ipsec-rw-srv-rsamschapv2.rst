=====================================================
IPsec: Setup OPNsense for IKEv2 Mutual RSA + MSCHAPv2
=====================================================

.. contents:: Index

Mutual RSA + MSCHAPv2 via IKEv2 is based on client certificate authentication combined with username
and password via MSCHAPv2.
Be sure that the client certificate is installed on your users device.

----------------------------
Step 1 - Create Certificates
----------------------------

For Mutual RSA + MSCHAPv2 with IKEv2 you need to create a Root CA and a server certificate
for your Firewall.

For more information read `Setup Self-Signed Certificate Chains </manual/how-tos/self-signed-chain.html>`_

-----------------------
Step 2 - Mobile Clients
-----------------------
First we will need to setup the mobile clients network and authentication source.
Go to :menuselection:`VPN --> IPsec --> Mobile Clients`

For our example will use the following settings:

IKE Extensions
--------------
========================== ================ =============================================
 **Enable**                 checked          *check to enable mobile clients*
 **User Authentication**    Local Database   *For the example we use the Local Database*
 **Group Authentication**   none             *Leave on none*
 **Virtual Address Pool**   10.10.0.0/24     *Enter the IP range for the remote clients*
========================== ================ =============================================

You can select other options, but we will leave them all unchecked for this example.

**Save** your settings and select **Create Phase1** when it appears.
Then enter the Mobile Client Phase 1 setting.

-------------------------------
Step 3 - Phase 1 Mobile Clients
-------------------------------

Phase 1 General information
---------------------------
========================== ============= ==================================================
 **Connection method**      default       *default is 'Start on traffic'*
 **Key Exchange version**   V2            *only V2 is supported for this type*
 **Internet Protocol**      IPv4
 **Interface**              WAN           *choose the interface connected to the internet*
 **Description**            MobileIPsec   *freely chosen description*
========================== ============= ==================================================

Phase 1 proposal (Authentication)
---------------------------------
=========================== ======================= ============================================
 **Authentication method**   Mutual RSA + MSCHAPv2  *This is the method we want here*
 **My identifier**           Distinguished Name     *Set the FQDN you used within certificate*
 **My Certificate**          Certificate            *Choose the certificate from dropdown list*
=========================== ======================= ============================================


Phase 1 proposal (Algorithms)
-----------------------------
========================== ================ ============================================
 **Encryption algorithm**   AES              *For our example we will use AES/256 bits*
 **Hash algoritm**          SHA1, SHA256     *SHA1 and SHA256 for compatibility*
 **DH key group**           1024, 2048 bit   *1024 and 2048 bit for compatibility*
 **Lifetime**               28800 sec        *lifetime before renegotiation*
========================== ================ ============================================

Advanced Options are fine by default.

**Save** your settings.

-------------------------------
Step 3 - Phase 2 Mobile Clients
-------------------------------

Press the button *+* in front of the phase 1 entry to add a new phase 2.

General information
-------------------
================= =============== =============================
 **Mode**          Tunnel IPv4     *Select Tunnel mode*
 **Description**   MobileIPsecP2   *Freely chosen description*
================= =============== =============================

Local Network
-------------
=================== ============ ==============================
 **Local Network**   LAN subnet   *Route the local LAN subnet*
=================== ============ ==============================

Phase 2 proposal (SA/Key Exchange)
----------------------------------
=========================== ============== ====================================================
 **Protocol**                ESP            *Choose ESP for encryption*
 **Encryption algorithms**   AES / 256      *For this example we use AES 256*
 **Hash algorithms**         SHA1, SHA256   *Same as before, mix SHA1 and SHA256*
 **PFS Key group**           off            *Most mobile systems do not support PFS in Phase2*
 **Lifetime**                3600 sec
=========================== ============== ====================================================

**Save** your settings and **Enable IPsec**, Select:

.. image:: images/ipsec_s2s_vpn_p1a_enable.png


.. Note::

   If you already had IPsec enabled and added Road Warrior setup, it is important to
   restart the whole service via services widget in the upper right corner of IPSec pages
   or via :menuselection:`System --> Diagnostics --> Services --> Strongswan` since applying configuration only
   reloads it, but a restart also loads the required modules of strongSwan.

------------------------
Step 4 - Add IPsec Users
------------------------

Go to :menuselection:`System --> Trust --> Certificates` and create a new client certificate.
Just click **Add**, choose your CA and probably increase the lifetime. Everything else besides
the CN can be left default. Give a **Common Name** and **Save**. Download the newly created
certificate as PKCS12 and export it to you enduser device.


Switch to :menuselection:`VPN -> IPsec -> Pre-Shared Keys` and press **Add**.
Enter the following into the form:

====================   ==========
 **Identifier**         expert
 **Pre-Shared Key**     &test!9T
 **Type**               EAP
====================   ==========
