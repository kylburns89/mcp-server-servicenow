# Troubleshooting

## Instance is hibernating
PDIs hibernate after inactivity. Wake it at [developer.servicenow.com](https://developer.servicenow.com) — click your instance and select "Wake Up". You'll get HTML login pages instead of JSON until it's awake.

## 401 Unauthorized
- **Basic auth:** Verify username/password are correct; check that the user has the `rest_api_explorer` or `admin` role
- **OAuth:** Ensure the OAuth Application Registry entry is active and the redirect URI matches. Try regenerating the client secret
- **Expired token:** OAuth tokens expire; the server retries once automatically, but if both attempts fail, check your credentials

## OAuth `get_current_user` returns different fields
With OAuth bearer tokens, `get_current_user` uses a Table API fallback (the UI endpoint requires session auth). The response fields are the same but sourced from `sys_user` instead of the UI API.

## CLI TypeError on stdio transport
Fixed in v0.3.1. If you see `TypeError: unexpected keyword argument 'host'`, upgrade: `pip install --upgrade mcp-server-servicenow`.
