# Publishing Security Checklist

Before committing an n8n workflow or screenshot:

- [ ] No API keys, Service Keys, OAuth tokens, signing secrets, or passwords.
- [ ] No private webhook URL, test URL, production URL, or query-string token.
- [ ] No customer names, emails, phone numbers, submissions, or raw execution output.
- [ ] Credentials are referenced only inside the private n8n account; public export contains no credential references.
- [ ] Slack channel IDs and workspace-specific identifiers are removed or replaced.
- [ ] Calendly links are intentional public links or placeholders.
- [ ] Workflow export is inactive and uses a placeholder webhook path.
- [ ] JSON parses successfully and can be imported into a separate test workflow.
- [ ] Every corrected node is retested before it is described as working.
- [ ] Commit diff reviewed before push.
