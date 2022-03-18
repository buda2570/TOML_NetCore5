# TOML_NetCore5
Create a .tmol file in .net core 5

I have been able to configure the power to create a .toml file in .Net Core 5 MVC.

TOML is a configuration file format that is easy to read due to obvious semantics that are intended to be "minimal". TOML is designed to unambiguously map a dictionary.

A use case is in Stellar to indicate information about an Asset which must be accessed in the following way

https://DOMAIN/.well-known/stellar.toml

and the content of the file is

********************************************
# Sample stellar.toml
VERSION="2.0.0"

NETWORK_PASSHRASE="Public Global Stellar Network ; September 2015"
FEDERATION_SERVER="https://api.domain.com/federation"
TRANSFER_SERVER="https://api.domain.com"
SIGNING_KEY="GBBHQ7H4V6RRORKYLHTCAWP6MOHNORRFJSDPXDFYDGJB2LPZUFPXUEW3"
HORIZON_URL="https://horizon.domain.com"
ACCOUNTS=[
"GD5DJQDDBKGAYNEAXU562HYGOOSYAEOO6AS53PZXBOZGCP5M2OPGMZV3",
"GAENZLGHJGJRCMX5VCHOLHQXU3EMCU5XWDNU4BGGJFNLI2EL354IVBK7",
"GAOO3LWBC4XF6VWRP5ESJ6IBHAISVJMSBTALHOQM2EZG7Q477UWA6L7U"
]

[DOCUMENTATION]
ORG_NAME="Organization Name"
ORG_DBA="Organization DBA"
ORG_URL="https://www.domain.com"
ORG_LOGO="https://www.domain.com/awesomelogo.png"
ORG_DESCRIPTION="Description of issuer"
ORG_PHYSICAL_ADDRESS="123 Sesame Street, New York, NY 12345, United States"
ORG_PHYSICAL_ADDRESS_ATTESTATION="https://www.domain.com/address_attestation.jpg"
ORG_PHONE_NUMBER="1 (123)-456-7890"
ORG_PHONE_NUMBER_ATTESTATION="https://www.domain.com/phone_attestation.jpg"
ORG_KEYBASE="accountname"
ORG_TWITTER="orgtweet"
ORG_GITHUB="orgcode"
ORG_OFFICIAL_EMAIL="support@domain.com"

[[MAIN]]
name="Jane Jedidiah Johnson"
email="jane@domain.com"
keybase="crypto_jane"
twitter="crypto_jane"
github="crypto_jane"
id_photo_hash="be688838ca8686e5c90689bf2ab585cef1137c999b48c70b92f67a5c34dc15697b5d11c982ed6d71be1e1e7f7b4e0733884aa97c3f7a339a8ed03577cf74be09"
verification_photo_hash="016ba8c4cfde65af99cb5fa8b8a37e2eb73f481b3ae34991666df2e04feb6c038666ebd1ec2b6f623967756033c702dde5f423f7d47ab6ed1827ff53783731f7"

[[CURRENCIES]]
code="ASDT"
issuer="GCZJM35NKGVK47BB4SPBDV25477PZYIYPVVG453LPYFNXLS3FGHDXOCM"
display_decimals=2

****************************************

In the MVC framework in .Net Core it is not possible to create a .toml file within the routing.
For this, the Static File system must be configured.

Startup.cs
************************************************** ********
var provide = new FileExtensionContentTypeProvider();
            provide.Mappings[".toml"] = "application/json";

            app.UseStaticFiles(
                new StaticFileOptions
                {
                    FileProvider = new PhysicalFileProvider(Path.Combine(Directory.GetCurrentDirectory(), ".well-known")),
                    RequestPath = "/.well-known",
                    ContentTypeProvider = provide
                }
            );
