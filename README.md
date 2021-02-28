# Archivist

## Running

Pretty technical and high-level for now.

Don't understand something? DM me @madamelic and I'll help you, then add here as I learn what needs more explaination.

- Deploy services you want to backup
  - You can find them [here](https://github.com/qnzl-archivist)
- If the service uses OAuth:
  - Create an OAuth app on the service
    - Make sure to set the OAuth callback to `{SERVICE URL}/api/oauth-callback`
  - Set your app's keys using the environment variables the corresponding @qnzl-archivist service uses
    - Typically `{SERVICE}_CLIENT_KEY` and `{SERVICE}_CLIENT_SECRET`
    - TODO: Make all of them consistent if possible
  - Authenticate by calling `/api/request-token`
    - This should redirect to the service to begin authentication then redirect to your service
  - When redirected to the service's OAuth callback, the page should display a JSON object with an access token and potentially a refresh token
    - Store these in a safe place and do not check them into source control
    - I use [@qnzl/lockbox](https://gitlab.com/qnzl/lockbox) but the best solution is Hashicorp's [Vault](https://www.vaultproject.io/)
- If the service uses API keys:
  - Create API keys on the service
  - Store API keys in a safe place
- Change `SERVICE_MAP` url in your `archiver-queue` script to point to your service
- Change `BLOCK_STORAGE_PATH` to point to where you want archives created
- Create a cronjob to run [archiver-queue](https://github.com/qnzl-archivist/archiver-queue)'s `start-dumb.js`' as often as you want to do backups
  - Check out https://cron.guru to write the approriate cron statement
  - Make sure to `source` the file you declared your environment variables in
