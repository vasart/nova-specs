==========================================
Nova Scheduler Extension
==========================================

Include the URL of your launchpad blueprint:

https://blueprints.launchpad.net/nova/+spec/periodic-security-checks-example

Introduction paragraph -- 
This blueprint introduces adapters between periodic security checks and component X in Nova, so that component X could notify and get check results from periodic security checks.

* For help with syntax, see http://sphinx-doc.org/rest.html

* To test out your formatting, build the docs using tox, or see:
  http://rst.ninjs.org



Problem description
===================

The security checks make the decision about whether a particular node is in the trusted pool or not. If the node passes check it is moved automatically to the trusted pool (if it’s not already there); if it fails the check, it is removed from the trusted pool.

Currently there is only one type of security checks implemented in OpenStack: static integrity checks performed by OpenAttestation component using TPM/IMA technology. And OpenAttestation check nodes only once, when the node is booted. Therefore, the first adapter would be implemented and connected between OpenAttestation and component X. And the future adapters would be easy to extend based on exsiting one.


Proposed change
===============

The adapters provide common interface that must be implemented by a check. The example interface from adapter implementation should be provided, which might be re-used by the developer to implement his/her own checks.

Alternatives
------------

There are other ways to solve this problem, for example Nova component connects the periodic security check component directly without adapter. But considering developer will implement his/her own checks in the future, above way would spend much more time and doesn't provide a good extensibility.

Data model impact
-----------------
None


REST API impact {TODO}
---------------

Each API method which is either added or changed should have the following

* Specification for the method

  * A description of what the method does suitable for use in
    user documentation

  * Method type (POST/PUT/GET/DELETE)

  * Normal http response code(s)

  * Expected error http response code(s)

    * A description for each possible error code should be included
      describing semantic errors which can cause it such as
      inconsistent parameters supplied to the method, or when an
      instance is not in an appropriate state for the request to
      succeed. Errors caused by syntactic problems covered by the JSON
      schema defintion do not need to be included.

  * URL for the resource

  * Parameters which can be passed via the url

  * JSON schema definition for the body data if allowed

  * JSON schema definition for the response data if any

* Example use case including typical API samples for both data supplied
  by the caller and the response

* Discuss any policy changes, and discuss what things a deployer needs to
  think about when defining their policy.

Example JSON schema definitions can be found in the Nova tree
http://git.openstack.org/cgit/openstack/nova/tree/nova/api/openstack/compute/schemas/v3

Note that the schema should be defined as restrictively as
possible. Parameters which are required should be marked as such and
only under exceptional circumstances should additional parameters
which are not defined in the schema be permitted (eg
additionaProperties should be False).

Reuse of existing predefined parameter types such as regexps for
passwords and user defined names is highly encouraged.

Security impact
---------------
None

Notifications impact
--------------------

This solution will change the trusted filter in Nova, so far trusted filter contains the connection function between OpenAttestation and Nova. We need remove the exsiting function.

Other end user impact 
---------------------
None


Performance Impact
------------------
None

Other deployer impact
---------------------
None

Developer impact
----------------
None


Implementation
==============

Assignee(s)
-----------

Who is leading the writing of the code? Or is this a blueprint where you're
throwing it out there to see who picks it up?

If more than one person is working on the implementation, please designate the
primary author and contact.

Primary assignee:
  <launchpad-id or None>

Other contributors:
  <launchpad-id or None>

Work Items
----------

The related work items for this project includes a Horizon component, peridic security checks.

We aim to merge this feature as a part of the Juno release of OpenStack.

Dependencies 
{TODO: include Nova-2nd part link. Is OA part of this or next one? IMA/TPM?}
============
* Horizon Component:
  https://blueprints.launchpad.net/horizon/+spec/periodic-security-checks

* Nova Component:
  https://blueprints.launchpad.net/nova/+spec/periodic-security-checks/


Testing
=======

Please discuss how the change will be tested. We especially want to know what
tempest tests will be added. It is assumed that unit test coverage will be
added so that doesn't need to be mentioned explicitly, but discussion of why
you think unit tests are sufficient and we don't need to add more tempest
tests would need to be included.

Is this untestable in gate given current limitations (specific hardware /
software configurations available)? If so, are there mitigation plans (3rd
party testing, gate enhancements, etc).


Documentation Impact
====================

What is the impact on the docs team of this change? Some changes might require
donating resources to the docs team to have the documentation updated. Don't
repeat details discussed above, but please reference them here.


References
==========

Please add any useful references here. You are not required to have any
reference. Moreover, this specification should still make sense when your
references are unavailable. Examples of what you could include are:

* Links to mailing list or IRC discussions

* Links to notes from a summit session

* Links to relevant research, if appropriate

* Related specifications as appropriate (e.g.  if it's an EC2 thing, link the
  EC2 docs)

* Anything else you feel it is worthwhile to refer to
