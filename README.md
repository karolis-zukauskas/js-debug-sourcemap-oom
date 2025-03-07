# js-debug-sourcemap-oom

Minimal reproduction for issue: [vscode-js-debug/Crash (OOM) on debugging](https://github.com/microsoft/vscode-js-debug/issues/2173).

This seems to reproduce in VSCode and Chrome dev tools. The larger the source maps, the faster memory grows.

Tested with:
- Windows 11
- VSCode 1.98.0

## Steps to reproduce

For VSCode:
1. Open VSCode without extensions: `code --disable-extensions .`
1. Place breakpoint in `index.ts` *(not index.js)*, line 2 *(inside setInterval)*
1. Start `Lorem Ipsum` VSCode launch target (hit F5)
1. Once breakpoint hits, stop debugging (shift-F5) and start over
1. Notice that memory of VSCode extension host process grows with each restart
1. Extension host crashes on reaching ~4-5 GB memory

**NOTE:**
If you delete inlined source maps from index.js and hit breakpoints there - no issues, memory is stable with each restart.

For Chrome dev tools:
1. Open Chrome `chrome://inspect` -> "Open dedicated DevTools for Node"
1. Configure connection ports if needed
1. Start process with `node --inspect=9222 index.js`
1. Place breakpoint in `index.ts` *(not index.js)*, line 2 *(inside setInterval)*
1. Once breakpoint hits, kill node process and restart it - chrome dev tools should auto attach to new process
1. Notice that memory of chrome dev tools process grows with each restart
1. Chrome dev tools crash on reaching ~4-5 GB memory
