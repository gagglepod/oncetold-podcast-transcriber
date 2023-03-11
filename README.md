# Oncetold Podcast Transcriber

_This is a clone of the [Modal Podcast Transcriber](https://github.com/modal-labs/modal-examples/tree/main/06_gpu_and_ml/whisper_pod_transcriber)_

This is a complete application that uses [OpenAI Whisper](https://github.com/openai/whisper) to transcribe podcasts. Modal spins up 100-300 containers for a single transcription run, so hours of audio can be transcribed on-demand in a few minutes.

- Modal.com LIVE App: You can find the app here: [https://modal-labs--whisper-pod-transcriber-fastapi-app.modal.run/](https://modal-labs--whisper-pod-transcriber-fastapi-app.modal.run/)
- Oncetold App: [https://gagglepod--whisper-pod-transcriber-fastapi-app.modal.run](https://gagglepod--whisper-pod-transcriber-fastapi-app.modal.run)

## Architecture

The entire application is hosted serverlessly on Modal and consists of 3 components:

1. React + Vite SPA ([`pod_transcriber/frontend/`](./pod_transcriber/frontend/))
2. FastAPI server ([`pod_transcriber/api.py`](./pod_transcriber/api.py))
3. Modal async job queue ([`pod_transcriber/main.py`](./pod_transcriber/main.py))

## Developing locally

### Requirements

- `account` approved [Modal.com](https://modal.com) account
- `npm`
- `modal` installed in your current Python virtual environment

### Podchaser Secret

To run this on your own Modal account, you'll need to [create a Podchaser account and create an API key](https://api-docs.podchaser.com/docs/guides/guide-first-podchaser-query/#getting-your-access-token).

Once you have the Podchaser account and API keys, then, create a [Modal Secret](https://modal.com/secrets/) with the following keys:

- `PODCHASER_CLIENT_SECRET`
- `PODCHASER_CLIENT_ID`

It doesn't matter what you call the Modal Secret block -- what matters is that both KEYS (with VALUES) are listed in the block (Note: This will not work locally).

You can find both on [their API page](https://www.podchaser.com/profile/settings/api).

### Vite build

`cd` into the `pod_transcriber/frontend` directory, and run:

- `npm install`
- `npx vite build --watch`

The last command will start a watcher process that will rebuild your static frontend files whenever you make changes to the frontend code.

### Serve on Modal

Once you have `vite build` running, in a separate shell run this at the app root to start an ephemeral app on Modal:

```shell
modal serve oncetold-podcast-transcriber.main
```

Pressing `Ctrl+C` will stop your app.

### Deploy to Modal

Once your happy with your changes, cd to the root of the project and run `modal deploy oncetold-podcast-transcriber.main` to deploy your app to Modal.

### Testing

Modal.com deployment allows you to transcribe HTML only at about $.15/60-minutes of audio. However, you can cut and paste what looks like an SRT version of the transcript to your own \*.SRT file.
