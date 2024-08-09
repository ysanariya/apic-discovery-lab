# apic-discovery-lab

The Apic Discovery Lab repo allows you to test the discovery action from [here](https://github.com/ibm-apiconnect/apic-discovery-action). This is an example repo from where the API documents can be sent to the API connect manager using the discovery action and where they can be promoted as required to be managed by Apiconnect through their entire lifecycle.

See [discover-api.yml](.github/workflows/discover-api.yml) and [send-to-discovery.yml](.github/workflows/send-to-discovery.yml)

The file `discover-api.yml` has an example of sending the API documents in different ways to the host `d-j02.apiconnect.dev.automation.ibm.com` to the provider organisation name `niraimathi` when the [workflow file](.github/workflows/discover-api.yml) or any one of the mentioned API file content changes.<br /> 

The job `check_changes_job` checks for the changes in the workflow file or mentioned API document and the job `run-discovery` sends the documents mentioned.<br /> 
The `run-discovery` job uses the apic-discovery-action repo main branch in the ibm-apiconnect organization
 - uses: ibm-apiconnect/apic-discovery-action@main <br /> 
with the parameters supplied
```
with:
  api_host: ${{ env.API_HOST }}
  platform_api_prefix: ${{ env.PLATFORM_API_PREFIX }}
  provider_org: ${{ env.PROVIDER_ORG }}
  api_key: ${{ secrets.apicApikey }}
  if: env.API_FILES
  api_files: ${{ env.API_FILES }}
  else if: env.API_FOLDERS
  api_folders: ${{ env.API_FOLDERS }}
  resync_check: ${{ needs.check_changes_job.outputs.action_changed && true || false }}
```
The file `send-to-discovery.yml` is another example of sending API documents with the extra variable **`git_diff`** which sends the git diff files between the current and previous commit. And the discovery action will send the `api_files or api_folders` to the server when there is a change in the files.

The content can be modified according to the test requirement for sending the APIs

Note: When an API with a large payload (complex API more than 1 Mb in size) is sent, there is expected to be some delay in creating the discovered API. It is recommended not to run another workflow simultaneously until you see an API has been created in the discovery service. 
