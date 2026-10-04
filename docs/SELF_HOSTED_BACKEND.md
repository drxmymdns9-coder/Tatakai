# Self-hosted Tatakai backend

This fork contains the frontend, Supabase migrations, and the `send-notification` Edge Function. It does not contain the private production TatakaiAPI instance or the original project's data.

## 1. Create a Supabase project

Create a new project in the Supabase dashboard. Keep the database password safe; it is needed once by the migration workflow.

Add these GitHub repository secrets:

| Secret | Value |
|---|---|
| `SUPABASE_ACCESS_TOKEN` | Personal access token from Supabase account settings |
| `SUPABASE_PROJECT_REF` | The new project's reference ID |
| `SUPABASE_DB_PASSWORD` | The new project's database password |
| `VITE_SUPABASE_URL` | `https://<project-ref>.supabase.co` |
| `VITE_SUPABASE_ANON_KEY` | The project's public/anon or publishable key |

Never add a `service_role` key to the iOS build secrets. Never commit any of these values to the repository.

## 2. Apply the database schema

Open GitHub → Actions → **Deploy Tatakai Supabase backend** → **Run workflow**. The workflow links the new project, applies every migration in `supabase/migrations`, and deploys `send-notification`.

## 3. Build iOS

After the backend workflow succeeds, run **Build Tatakai iOS**. The build workflow validates `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` before compiling, so it cannot silently produce the previous broken IPA.

## Compatibility scope

The new project has the same schema, Row Level Security policies, RPC functions, storage definitions, and realtime declarations represented by the committed migrations. It starts with no users or community content. Anime/manga metadata, streaming providers, OAuth integrations, and the private TatakaiAPI services still need their own configuration or replacement endpoints.
