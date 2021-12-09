# shrawan
No
name:Shrawan achraya
on:
  push:
    branches:
      - main

env:
  EM_VERSION: 2.0.10
  EM_CACHE_FOLDER: 'emsdk-cache'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v2.3.1
        with:
          persist-credentials: false
      - name: ffmpeg
        uses: FedericoCarboni/setup-ffmpeg@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Setup EMSDK Cache 🔧
        id: cache-system-libraries
        uses: actions/cache@v2
        with:
          path: ${{env.EM_CACHE_FOLDER}}
          key: ${{env.EM_VERSION}}-${{ runner.os }}

      - uses: mymindstorm/setup-emsdk@v7
        with:
          version: ${{env.EM_VERSION}}
          actions-cache-folder: ${{env.EM_CACHE_FOLDER}}

      - name: Install and Build 🔧
        run: |
          yarn
          yarn build

      - name: Deploy ☻
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
