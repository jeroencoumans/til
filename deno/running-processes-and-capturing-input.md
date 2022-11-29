I want to write a Deno script that will call an external program and capture its output. This is possible!

In the REPL, it works great:

```shellsession
$ deno repl
Deno 1.26.1
exit using ctrl+d, ctrl+c, or close()
> const p = Deno.run({
    cmd: ['echo', 'hello world'],
    stderr: 'piped',
    stdout: 'piped',
  })
undefined
> let output = await p.output()
undefined
> let status = await p.status()
undefined
> output
Uint8Array(12) [
  104, 101, 108, 108,
  111,  32, 119, 111,
  114, 108, 100,  10
]
> status
{ success: true, code: 0 }
> let str = new TextDecoder().decode(output)
undefined
> str
"hello world\n"
> p.close()
undefined
>
```

However, in Deno.test, you need to run the command in a separate function like this:

```js

Deno.test('Echo works', async (t) => {
  let input = 'Hello world';
  let expected = `${input}\n`;
  let actual = await echo(input);
  assert(expected === actual);
});

async function echo(input) {
  // run the command
  const p = Deno.run({
    cmd: ['echo', input],
    stdout: 'piped',
  });
  // wait until it's completed
  let status = await p.status();
  p.close();

  if (status.success) {
    return new TextDecoder().decode(await p.output());
  } else {
    let error = new TextDecoder().decode(await p.stderr());
    throw new Error(error);
  }
}
```

Not sure why this is the case, but somehow the output would be an empty string if the command body is part of the async test.