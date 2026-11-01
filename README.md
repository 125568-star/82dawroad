# 82 Daw Road Community Portal

Community website for the 82 Daw Road townhouse complex in Runcorn, Queensland.

- Public landing page
- Supabase email/password authentication
- Coordinator-approved members with roles: resident, owner, body_corp, coordinator
- Private notices and maintenance requests
- Coordinator member approval and role management

## Website

This repository serves a single static `index.html` file and can be deployed as GitHub Pages or under `https://groundsupportcrew.com/82DawRoad/`.

**Deployment to groundsupportcrew.com is separate from this repository commit.** Configure hosting or synchronize this repository into the main site's `82DawRoad` directory.

## Supabase

Project: **Coordinator** (ref: `pulbcpjwxliwnmkiyfhe`). The database tables and policies have been created with migration `create_daw82_community_portal`:

- `public.daw82_profiles`
- `public.daw82_notices`
- `public.daw82_requests`
- `daw82_private.has_role` security helper

Only a publishable public API key is used in the browser. Never put service-role or secret keys into GitHub.

### First coordinator activation

1. Sign up on the website using the intended coordinator email; verify the email and sign in.
2. The account will appear in `public.daw82_profiles` with `approved=false`.
3. In the Supabase SQL editor, after verifying the exact account email belongs to the authorized coordinator, run:

```sql
update public.daw82_profiles p
set role='coordinator', approved=true
from auth.users u
where p.user_id = u.id
  and u.email = 'REPLACE_WITH_VERIFIED_COORDINATOR_EMAIL';
```

Do not enable public self-assignment of privileged roles. From then on, the coordinator approves other members in the dashboard.

### Important privacy and operations notes

The repository is public but private member information lives in Supabase behind Row Level Security. Verify Auth email confirmation settings, appropriate redirect URLs, and Supabase configuration before inviting real residents. Keep actual resident details and body corporate documents out of GitHub. The website does not yet include document uploads; those require a private storage bucket and storage access policies.

