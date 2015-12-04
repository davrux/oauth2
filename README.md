# OAuth2 for Go

This package is a fork of the original oauth2 package to support the
broken Microsoft Sharepoint OAuth implementation.

See [https://github.com/golang/oauth2](https://github.com/golang/oauth2) for the original
package.


## Installation

~~~~
go get github.com/davrux/oauth2
~~~~

## Usage

    import "github.com/davrux/oauth2"

    var sharepointConf = &oauth2.Config{
	    ClientID:     "client id",
	    ClientSecret: "secret key",
	    Scopes: []string{
		    "Web.Read",
		    "Web.Write",
		    "Web.Manage",
		    "List.Read",
		    "List.Write",
		    "List.Manage",
		    "AllSites.Read",
		    "AllSites.Write",
		    "AllSites.Manage",
	    },
	    RedirectURL: "https://localhost/authorize",
	    Site:        "allgeieritsolutions.sharepoint.com",
    }

    // request Sharepoint realm
	err := sharepointConf.QuerySharepointRealm()
	if err != nil {
        ...
	}

    // set auth endpoints depending on site and realm
	sharepointConf.Endpoint = oauth2.SharepointOnlineEndpoint(sharepointConf.Site, sharepointConf.Realm)

    // Generate URL for OAuth. Let the user visit the site
	signinURL := sharepointConf.AuthCodeURL("state", oauth2.ApprovalForce)

    ...

    // In the handler for redirect URL
	tok, err := sharepointConf.ExchangeWithRealm(oauth2.NoContext, code)
	if err != nil {
        ...
	}

	client := sharepointConf.Client(oauth2.NoContext, tok)
	resp, err := client.Get(...)


## Register Sharepoint App

To register a Sharepoint App go to
https://your_site_name.com/_layouts/15/appregnew.aspx.
