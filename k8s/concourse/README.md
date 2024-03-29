# Concourse Basic Setup (non-production)

To get a basic installation of Concourse set up, along with Vault integration, do the following:

1. Run Vault in Kubernetes (out of scope for this file, see `../../vault/manual/README.md`)
2. Configure Vault with the following commands: 
   <br /><br />
   `$ vault secrets enable -version=1 -path=concourse kv` 
   <br />
   `$ vault policy write concourse ./concourse-policy.hcl`
   <br />
   `$ vault token create --policy concourse --period 1h`
   <br /><br />
   The last command prints a table of key-value pairs generated by Vault.  Record the value of the row with key `token`.
   <br /><br />
   ***Note:*** `VAULT_ADDR` and `VAULT_TOKEN` may need to be set when issuing these commands.
3. Run PostgreSQL using supplied `pg_service.yaml` & `pg_deployment.yaml`:
   <br /><br />
   `$ kubectl apply -f pg_service.yaml`
   <br />
   `$ kubectl apply -f pg_deployment.yaml`
4. Input the `token` value retrieved in step 2 in `cc_deployment.yaml` under the `value` property for the `CONCOURSE_VAULT_CLIENT_TOKEN` environment variable
5. Run Concourse using the `cc_service.yaml` and `cc_deployment.yaml`:
   <br /><br />
   `$ kubectl apply -f cc_service.yaml`
   <br />
   `$ kubectl apply -f cc_deployment.yaml`

This setup is not appropriate for production, but will get a basic Concourse server set up, integrated with Vault for parameter substitution.

## Reference

[Concourse Integration with Vault](https://concourse-ci.org/vault-credential-manager.html)
