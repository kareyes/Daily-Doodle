
### Links
- [YT Guide](https://www.youtube.com/watch?v=kZYsoav104w)
- [--allow-unauthenticated permission](https://cloud.google.com/functions/docs/securing/managing-access-iam#allowing_unauthenticated_http_function_invocation)

[Direct Workload Identity Fedaration](https://github.com/google-github-actions/auth)
[YT - Guide](https://www.youtube.com/watch?v=jNb2CFsHjsY&ab_channel=CloudAdvocate)

1. Create a Workload Identity Pool
```
gcloud iam workload-identity-pools create "github" \
  --project="${PROJECT_ID}" \
  --location="global" \
  --display-name="GitHub Actions Pool" 
```

```
gcloud iam workload-identity-pools create "github" \
  --project="maze-461304" \
  --location="global" \
  --display-name="GitHub Actions Pool"
```
2. Get Full ID of the Workload Identity Pool
```
gcloud iam workload-identity-pools describe "github" \
  --project="${PROJECT_ID}" \
  --location="global" \
  --format="value(name)"

```

```
gcloud iam workload-identity-pools describe "github" \
  --project="maze-461304" \
  --location="global" \
  --format="value(name)"
```

```
projects/723726653996/locations/global/workloadIdentityPools/github
```
3. Create a Workload Identity **Provider** in that pool
```
# TODO: replace ${PROJECT_ID} and ${GITHUB_ORG} with your values below.

gcloud iam workload-identity-pools providers create-oidc "my-repo" \
  --project="${PROJECT_ID}" \
  --location="global" \
  --workload-identity-pool="github" \
  --display-name="My GitHub repo Provider" \
  --attribute-mapping="google.subject=assertion.sub,attribute.actor=assertion.actor,attribute.repository=assertion.repository,attribute.repository_owner=assertion.repository_owner" \
  --attribute-condition="assertion.repository_owner == '${GITHUB_ORG}'" \
  --issuer-uri="https://token.actions.githubusercontent.com"
```

```
gcloud iam workload-identity-pools providers create-oidc "my-repo" \
  --project="maze-461304" \
  --location="global" \
  --workload-identity-pool="github" \
  --display-name="My GitHub repo Provider" \
  --attribute-mapping="google.subject=assertion.sub,attribute.actor=assertion.actor,attribute.repository=assertion.repository,attribute.repository_owner=assertion.repository_owner" \
  --attribute-condition="assertion.repository_owner == 'broldansaztrek'" \
  --issuer-uri="https://token.actions.githubusercontent.com"
```
4. Extract the Workload Identity **Provider** resource name:
```
# TODO: replace ${PROJECT_ID} with your value below.

gcloud iam workload-identity-pools providers describe "my-repo" \
  --project="${PROJECT_ID}" \
  --location="global" \
  --workload-identity-pool="github" \
  --format="value(name)"
```

```
gcloud iam workload-identity-pools providers describe "my-repo" \
  --project="maze-461304" \
  --location="global" \
  --workload-identity-pool="github" \
  --format="value(name)"
```

```
projects/723726653996/locations/global/workloadIdentityPools/github/providers/my-repo
```

5. manually create a secret on your secret manager in GCP `my-secret`

6. Principal Set Setup
```
# TODO: replace ${PROJECT_ID}, ${WORKLOAD_IDENTITY_POOL_ID}, and ${REPO}
# with your values below.
#
# ${REPO} is the full repo name including the parent GitHub organization,
# such as "my-org/my-repo".
#
# ${WORKLOAD_IDENTITY_POOL_ID} is the full pool id, such as
# "projects/123456789/locations/global/workloadIdentityPools/github".

gcloud secrets add-iam-policy-binding "my-secret" \
  --project="${PROJECT_ID}" \
  --role="roles/secretmanager.secretAccessor" \
  --member="principalSet://iam.googleapis.com/${WORKLOAD_IDENTITY_POOL_ID}/attribute.repository/${REPO}"
```

```
gcloud secrets add-iam-policy-binding "my-secret" \
  --project="maze-461304" \
  --role="roles/secretmanager.secretAccessor" \
  --member="principalSet://iam.googleapis.com/projects/723726653996/locations/global/workloadIdentityPools/github/attribute.repository/broldansaztrek/ouroboroslaikaprojects"
```
5. Update github action YML
```
# add permission after - runs-on: ubuntu-latest

runs-on: ubuntu-latest
permissions:
      contents: 'read'
      id-token: 'write'
```

```
# Update auth

- name: Google Cloud Auth

     uses: 'google-github-actions/auth@v2'

     with:

      project_id: ${{ env.PROJECT_ID }}  

      workload_identity_provider: 'projects/723726653996/locations/global/workloadIdentityPools/github/providers/my-repo'
```
5. on IAM - prep access
	1. Grant Access Button
		1. New Principals - principalSet://iam.googleapis.com/projects/723726653996/locations/global/workloadIdentityPools/github/attribute.repository/broldansaztrek/ouroboroslaikaprojects
		2. Add Roles
			1. Cloud Run Admin
			2. Artifact Registry Writer
			3. Service Account User

grant access 
actor = maze-repo-actor