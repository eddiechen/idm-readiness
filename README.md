# Red Hat Identity Management Readiness Questionnaire

## Clients and Topology
* What versions of RHEL are you using today?
* Roughly how many RHEL systems will be integrated into IdM?
* How many geopgraphic sites (e.g., data centers) house RHEL systems, and what is the connectivity between them?
* Are there non-RHEL systems (e.g., other Unix or Linux) that will be leveraging IdM for users and groups?

## DNS
* IdM requires a unique and dedicated namespace (DNS domain, LDAP context, Kerberos Realm) that does not overlap with any other solution (like 
* Will you be using existing DNS server or DNS service hosted by IDM?
* If using existing DNS server, can SRV records be entered into DNS of the form ”_ldap._tcp SRV 1 49 389 idm.example.com”

## Active Directory Integration
* What version of Microsoft Windows is AD running on today? (IDM supports Windows 2008R2 and 2012)
* Has the Active Directory team been consulted on establishing a cross-forest trust between IdM and AD?
  - Currently with RHEL 7.1, IDM can only create a 2-way trust with AD domain
  - Even though it is a 2-way trust, IDM does not have the components to allow AD to trust IDM. It is an informal 1-way trust
  - In order to create the trust during IDM configuration, an AD admin credential is needed. Or AD admin can generate a Kerberos trust secret certificate
  - Explicit 1-way and 2-way trusts is on the roadmap
  - Global catalog service to allow AD to lookup IDM credentials, required by a true 2-way trust, is on the roadmap.
* On Average, how many groups does an AD user belong to
  - We have seem as high as over 200 groups per AD user.  It hindered login performance. SSSD had to read every group and timed out before it could finish.  If large number of memberships, please consult with an IdM SME before proceed further
  - AD user with groups 100+ and groups being big and nested some optimization might be needed. So do not expect that SSSD would blindly work without any performance issues. Default configuration is not optimized for large numbers of groups and SSD tune up would be due.
* Are POSIX attributes populated inside AD (uid,gid,HomeDir,loginshell)
  - It is recommended to replicate them to the global catalog for better performance
  - SSSD is able to autodetect if POSIX attributes are not present in GC and disable the GC support on the fly
  - Use the "--range-type" parameter on "ipa trust-add" to specify that POSIX attributes should be read from AD
  - https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/ex.sssd-ad-posix.html


