

Deploy to alpha hosting

npm run build

firebase deploy --only hosting:mermade-dev-delta

firebase deploy --only hosting:mermade-dev-alpha

Add hosting sites:

firebase hosting:sites:create [SITE_ID]