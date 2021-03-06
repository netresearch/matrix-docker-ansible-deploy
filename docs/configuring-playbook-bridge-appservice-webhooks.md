# Setting up Appservice Webhooks (optional)

The playbook can install and configure [matrix-appservice-webhooks](https://github.com/turt2live/matrix-appservice-webhooks) for you.

Setup Instructions:

loosely based on [this](https://github.com/turt2live/matrix-appservice-webhooks/blob/master/README.md)

1. All you basically need is to adjust your `inventory/host_vars/matrix.<domain-name>/vars.yml`:
    ```yaml
    matrix_appservice_webhooks_enabled: true
    matrix_appservice_webhooks_api_secret: '<your_secret>'
    ```

2. In case you want to change the verbosity of logging via `journalctl -fu matrix-appservice-webhooks.service`
you can adjust this in `inventory/host_vars/matrix.<domain-name>/vars.yml` as well.
<br>
*Note*: default value is: `info` and availabe log levels are : `info, verbose`

    ```yaml
    matrix_appservice_webhooks_log_level: '<log_level>'
    ```

3. If you've already installed Matrix services using the playbook before, you'll need to re-run it (`--tags=setup-all,start`). If not, proceed with [configuring other playbook services](configuring-playbook.md) and then with [Installing](installing.md). Get back to this guide once ready.

4. Invite the bridge bot user to your room:
    - either with:
      ```
      /invite @_webhook:<domain.name>
      ```
        *Note*: Make sure you have administration permissions in your room
    - or simply add the bridge bot to a private channel (personal channels imply you being an administrator)

5. Send a message to the bridge bot in order to receive a private message including the webhook link.
    ```
    !webhook
    ```

6. The JSON body for posting messages will have to look like this:
    ```
    {
        "text": "Hello world!",
        "format": "plain",
        "displayName": "My Cool Webhook",
        "avatarUrl": "http://i.imgur.com/IDOBtEJ.png"
    }
    ```
   - You can test this via curl like so:
     ```
     curl --header "Content-Type: application/json" \
     --data '{
     "text": "Hello world!",
     "format": "plain",
     "displayName": "My Cool Webhook",
     "avatarUrl": "http://i.imgur.com/IDOBtEJ.png"
     }' \
     <the link you've gotten in 5.>
     ```
