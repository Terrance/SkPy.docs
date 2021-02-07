Credentials
===========

Skype for Web makes use of two types of credential: a Skype token (obtained through an authentication flow in exchange for a username and password) used by most of the meta endpoints, and a registration token (obtained using the Skype token) specifically for messaging interactions.

Unless otherwise noted, authentication is handled as follows:

- APIs on ``client-s.gateway.messenger.live.com`` (or an alternative subdomain, see :ref:`Registration token`) require registration token authentication using the ``RegistrationToken`` header.
- APIs on ``api.asm.skype.com`` take an ``Authorization`` header of the form ``skype_token <token>``.
- All other APIs take an ``X-SkypeToken`` header set to the Skype token.

Some of the known response codes related to authentication:

- HTTP 429, error code 803: auth rate limit exceeded (-5 minute cooldown)
- HTTP 404, error code 729: no endpoint created (need to refresh registration token)

Skype token
-----------

Live authentication
~~~~~~~~~~~~~~~~~~~

Authentication with either a Skype username or a Microsoft account requires calling out to the MS OAuth page, and retrieving the Skype token.

.. http:get:: https://login.skype.com/login/oauth/microsoft

    This will redirect to ``login.live.com``.  Collect the value of the hidden field named ``PPFT``.

    :query client_id: ``578134``
    :query redirect_uri: ``https://web.skype.com``
    :resheader Cookie: contains ``MSPRequ`` and ``MSPOK``, both required for the next step

.. http:post:: https://login.live.com/ppsecure/post.srf

    If all is well, a hidden field with identifier ``t`` will contain a token for the last step.

    :query wa: ``wsignin1.0``
    :query wp: ``MBI_SSL``
    :query wreply: ``https://lw.skype.com/login/oauth/proxy?client_id=578134&site_name=lw.skype.com&redirect_uri=https%3A%2F%2Fweb.skype.com%2F``
    :reqheader Set-Cookie: include ``MSPRequ`` and ``MSPOK`` as obtained earlier, and ``CkTst`` with a timestamp in the standard format
    :form login: Skype username or Microsoft account email address
    :form passwd: corresponding account password
    :form PPFT: as obtained from the hidden field

.. http:post:: https://web.skype.com/login/microsoft

    The Skype token and expiry can be retrieved in the same fields as with a username/password login.

    :query client_id: ``578134``
    :query redirect_uri: ``https://web.skype.com``
    :form client_id: ``578134``
    :form redirect_uri: ``https://web.skype.com``
    :form oauthPartner: ``999``
    :form site_name: ``lw.skype.com``
    :form t: as obtained earlier

SOAP authentication
~~~~~~~~~~~~~~~~~~~

Authentication with a Microsoft account email address and password (or application-specific token), using an endpoint to obtain a security token, and exchanging that for a Skype token,

.. http:post:: https://login.live.com/RST.srf

    This is an XML endpoint that will exchange a Microsoft account email address and password for a security token.

    Request body:

    .. code-block:: xml

        <Envelope xmlns="http://schemas.xmlsoap.org/soap/envelope/"
           xmlns:wsse="http://schemas.xmlsoap.org/ws/2003/06/secext"
           xmlns:wsp="http://schemas.xmlsoap.org/ws/2002/12/policy"
           xmlns:wsa="http://schemas.xmlsoap.org/ws/2004/03/addressing"
           xmlns:wst="http://schemas.xmlsoap.org/ws/2004/04/trust"
           xmlns:ps="http://schemas.microsoft.com/Passport/SoapServices/PPCRL">
           <Header>
               <wsse:Security>
                   <wsse:UsernameToken Id="user">
                       <wsse:Username>...</wsse:Username>
                       <wsse:Password>...</wsse:Password>
                   </wsse:UsernameToken>
               </wsse:Security>
           </Header>
           <Body>
               <ps:RequestMultipleSecurityTokens Id="RSTS">
                   <wst:RequestSecurityToken Id="RST0">
                       <wst:RequestType>http://schemas.xmlsoap.org/ws/2004/04/security/trust/Issue</wst:RequestType>
                       <wsp:AppliesTo>
                           <wsa:EndpointReference>
                               <wsa:Address>wl.skype.com</wsa:Address>
                           </wsa:EndpointReference>
                       </wsp:AppliesTo>
                       <wsse:PolicyReference URI="MBI_SSL"></wsse:PolicyReference>
                   </wst:RequestSecurityToken>
               </ps:RequestMultipleSecurityTokens>
           </Body>
        </Envelope>

    Response body (token under ``BinarySecurityToken``):

    .. code-block:: xml

        <?xml version="1.0" encoding="utf-8" ?>
        <S:Envelope
            xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
            <S:Header></S:Header>
            <S:Body>
                <wst:RequestSecurityTokenResponseCollection
                    xmlns:S="http://schemas.xmlsoap.org/soap/envelope/"
                    xmlns:wst="http://schemas.xmlsoap.org/ws/2004/04/trust"
                    xmlns:wsse="http://schemas.xmlsoap.org/ws/2003/06/secext"
                    xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd"
                    xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
                    xmlns:wsp="http://schemas.xmlsoap.org/ws/2002/12/policy"
                    xmlns:psf="http://schemas.microsoft.com/Passport/SoapServices/SOAPFault">
                    <wst:RequestSecurityTokenResponse>
                        <wst:TokenType>urn:passport:compact</wst:TokenType>
                        <wsp:AppliesTo
                            xmlns:wsa="http://schemas.xmlsoap.org/ws/2004/03/addressing">
                            <wsa:EndpointReference>
                                <wsa:Address>wl.skype.com</wsa:Address>
                            </wsa:EndpointReference>
                        </wsp:AppliesTo>
                        <wst:LifeTime>
                            <wsu:Created>2021-01-01T12:00:00Z</wsu:Created>
                            <wsu:Expires>2021-01-02T12:00:00Z</wsu:Expires>
                        </wst:LifeTime>
                        <wst:RequestedSecurityToken>
                            <wsse:BinarySecurityToken Id="Compact0">...</wsse:BinarySecurityToken>
                        </wst:RequestedSecurityToken>
                        <wst:RequestedTokenReference>
                            <wsse:KeyIdentifier ValueType="urn:passport:compact"></wsse:KeyIdentifier>
                            <wsse:Reference URI="#Compact0"></wsse:Reference>
                        </wst:RequestedTokenReference>
                    </wst:RequestSecurityTokenResponse>
                </wst:RequestSecurityTokenResponseCollection>
            </S:Body>
        </S:Envelope>

.. http:post:: https://edge.skype.com/rps/v1/rps/skypetoken

    Convert the Microsoft security token into a Skype token.

    :reqjson partner: ``999``
    :reqjson scopes: ``client``
    :reqjson access_token: token from above
    :resjson skypetoken: resulting Skype token
    :resjson skypeid: username of the authenticated user
    :resjson signinname: identifier of the linked Microsoft account
    :resjson expiresIn: number of seconds until the token expires

Guest access
~~~~~~~~~~~~

Skype also supports the notion of a guest, who can access a conversation from an invite, without a Skype account.

A guest account differs from regular accounts in that:

- They can only access a single group conversation.
- Their username is prefixed with ``guest:``.
- They have no profile information, just a display name.
- They expire after 24 hours.

.. http:get:: https://join.skype.com/(string:id)

    :param id: public join URL code
    :reqheader User-Agent: must be set to that of a supported device, e.g. Chrome
    :resheader Set-Cookie: CSRF token in ``csrf_token``, request identifier in ``launcher_session_id``

.. http:post:: https://join.skype.com/api/v1/users/guests

    :reqheader csrf_token: as above
    :reqheader X-Skype-Request-Id: session identifier from above
    :reqjson flowId: session identifier from above
    :reqjson shortId: public join URL code
    :reqjson longId: identifier retrieved from join.skype.com URL lookup
    :reqjson threadId: chat identifier (``19:<random>@thread.skype``)
    :reqjson name: guest display name
    :resheader Set-Cookie: token cookie named ``guest_token_<thread>`` containing the new token

Registration token
------------------

.. http:post:: https://client-s.gateway.messenger.live.com/v1/users/ME/endpoints

    .. note:: A JSON object must be provided in the body of the request, even if empty.

    The non-standard header ``LockAndKey`` is required, and has the following format::

        appId=msmsgs@msnmsgr.com; time=<timestamp>; lockAndKeyResponse=...

    Here, ``time`` is a UNIX timestamp in the same format as before.  The actual response must be generated through some Skype-specific crypto -- see :meth:`skpy.conn.getMac256Hash` for the algorithm.

    In some cases, a call to this endpoint will return a ``Location`` header pointing to a different subdomain (e.g. ``https://db1-client-s.gateway.messenger.live.com``.  In this case, repeat the call using the new URL.  You should use this domain in place of the default one for all other gateway calls.

    :reqheader Authentication: Skype token in the form ``skypetoken=<token>``
    :reqheader LockAndKey: key response as above
    :resheader Location: URL to newly generated endpoint, or to required subdomain
    :resheader Set-RegistrationToken: token response in the form ``registrationToken=<token>; expires=<timestamp>; endpointId=<id>``
