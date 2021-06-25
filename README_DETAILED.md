# Bitbucket Script

## Create New Bitbucket Consumer
Go to https://bitbucket.org/anmacode/workspace/settings/api
Click Add consumer
Set Consumer's Name, Callback URL (that will do something with passed in query params)
Check the Read Repositories,  Read Projects, Read Accout permissions

Your consumer should have a Key and Secret
Key:  {consumer_key}

Secret:  {consumer_secret}

---
Note: https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/

We will do implicit grant, so insert in your consumer's key into this link
https://bitbucket.org/site/oauth2/authorize?client_id={key}&response_type=token

https://bitbucket.org/site/oauth2/authorize?client_id={consumer_key}&response_type=token


And go to this link in the browser. You should get a permission prompt, and then it should redirect you to


{CALLBACK_URL}/#access_token={access_token}&scopes=repository&expires_in=7200&token_type=bearer



Extract from URL

---

When making requests, send access token in request header
1. `Authorization: Bearer {access token}`
2. Include in application/x-www-form-urlencoded POST body as `access_token={access_token}`
3. Put in query string of non-POST: `?access_token={access_token}`


Refresh Tokens
- access tokens expire in 2 hours
- when tokens expire, you will get 401 responses
- most access token grant response also include a refresh token that can be used to generate a new access token, without needing end user participation
- `curl -X POST -u "client_id:secret" https://bitbucket.org/site/oauth2/access_token -d grant_type=refresh_token -d refresh_token={refresh_token}`


First get projects by workspace ID
https://api.bitbucket.org/2.0/workspaces/anmacode/projects

And res.values (is an array of project objects, and we can use projectObj.uuid)
We can loop through array of res.size (int), and then try next page res.page (int)



Returns a paginated list of all repositories owned by the specified account or UUID.
https://api.bitbucket.org/2.0/repositories/anmacode

res.values (array of repoObjects)
each repoObject has a repoObject.uuid (string)

---
{{API_URL}}/user

has userObj.nickname, userObj.uuid, userObj.has_2fa_enabled (null), has userObj.accout_id

---

Get list of all commits done by User
