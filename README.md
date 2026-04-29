# pi-nocchio

A tiny [Pi](https://github.com/badlogic/pi-mono) package that adds a CLI flag for printing Pi's assembled system prompt.

## What it does

`pi-nocchio` registers:

```bash
--dump-system-prompt
```

When the flag is present, Pi prints the turn-accurate assembled system prompt to stdout and exits before calling the model.

## Install

```bash
pi install npm:@robzolkos/pi-nocchio
```

You can also try it directly from GitHub:

```bash
pi -e git:github.com/robzolkos/pi-nocchio --dump-system-prompt
```

## Usage

Print the assembled system prompt for a dump turn:

```bash
pi --dump-system-prompt
```

Save it to a file:

```bash
pi --dump-system-prompt > system-prompt.txt
```

You can also provide your own initial prompt text:

```bash
pi --dump-system-prompt -p "dump" > system-prompt.txt
```

If no prompt is provided, `pi-nocchio` runs an internal synthetic `-p "dump"` turn so Pi still runs turn-scoped hooks like `before_agent_start`. The synthetic turn exits before any model request.

## Notes

- The flag exits before any model request is made.
- When `--dump-system-prompt` is not used, the extension only performs cheap flag checks in event hooks; practical performance impact is negligible.
- Output is captured while Pi builds the request context, after the full `before_agent_start` chain, so turn-scoped system-prompt changes from other extensions are included.
- If no prompt is provided, `pi-nocchio` runs a synthetic dump turn internally instead of printing a misleading startup-only prompt.
- The output includes Pi's loaded tools, context files, skills, custom system prompt, appended system prompt, current date, and current working directory as Pi assembles them.
- This prints Pi's system prompt string. It does not include later provider-payload rewrites from `before_provider_request` hooks.

## License

MIT
