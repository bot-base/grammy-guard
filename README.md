# Guard middlewares for grammY

## Introduction

A library that simplifies the creation of guard middlewares and contains a set of [predefined filters](https://github.com/bot-base/grammy-guard#reference) that are often used.

## Installation

### Node

```sh
npm install grammy-guard
```

or using `yarn`:

```sh
yarn add grammy-guard
```

### Deno

```ts
import { guard } from "https://deno.land/x/grammy_guard@v0.2.0/mod.ts";
```

## Example

```ts
import { Bot } from "grammy";
import {
    guard,
    isAdmin,
    isPrivate,
    isUserHasId,
    isUserHasUsername,
    not,
    or,
    reply,
} from "grammy-guard";

const bot = new Bot(process.env.BOT_TOKEN as string);

bot.command(
    "start",
    guard(isPrivate, reply("/start is only available in private chats!")),
    (ctx) => ctx.reply("Hello!"),
);

bot.command(
    "specificusers",
    guard(
        or(isUserHasId(1), isUserHasUsername("someusername")),
        reply("/specificusers is only available to the user with ID 1 or username @someusername!"),
    ),
    (ctx) => ctx.reply("Hello, user!"),
);

bot.command(
    "admin",
    guard(
        [not(isPrivate), isAdmin],
        reply("/admin is only available to administrators in non-private chats!"),
    ),
    (ctx) => ctx.reply("Hello, admin!"),
);

bot.start();
```

There's some more [examples](https://github.com/bot-base/grammy-guard/tree/main/examples):

- [Using context in reply](https://github.com/bot-base/grammy-guard/blob/main/examples/2-reply-context.ts) ([deno version](https://github.com/bot-base/grammy-guard/blob/main/examples/2-reply-context.deno.ts))
- [Custom guard handler](https://github.com/bot-base/grammy-guard/blob/main/examples/3-custom-handler.ts) ([deno version](https://github.com/bot-base/grammy-guard/blob/main/examples/3-custom-handler.deno.ts))
- [Custom guard predicate](https://github.com/bot-base/grammy-guard/blob/main/examples/4-custom-predicate.ts) ([deno version](https://github.com/bot-base/grammy-guard/blob/main/examples/4-custom-predicate.deno.ts))

## Reference

| Filter                                      | Description                                                                                                                                                             |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `isPrivate`                                 | Checks if it is a private chat                                                                                                                                          |
| `isBasicGroup`                              | Checks if it is a basic group chat                                                                                                                                      |
| `isSupergroup`                              | Checks if it is a supergroup chat                                                                                                                                       |
| `isGroup`                                   | Checks if it is a basic group or a supergroup chat                                                                                                                      |
| `isChannel`                                 | Checks if it is a channel                                                                                                                                               |
| `isChat`                                    | Checks if it is a chat                                                                                                                                                  |
| `isChatHasId(id: number)`                   | Checks if the chat has this ID                                                                                                                                          |
| `isChatHasUsername(username: string)`       | Checks if the chat has this username                                                                                                                                    |
| `isUser`                                    | Checks if the user is not a bot                                                                                                                                         |
| `isUserHasId(id: number)`                   | Checks if the user has this ID                                                                                                                                          |
| `isUserHasUsername(username: string)`       | Checks if the user has this username                                                                                                                                    |
| `isBot`                                     | Checks if the user is a bot                                                                                                                                             |
| `isBotHasId(id: number)`                    | Checks if the bot has this ID                                                                                                                                           |
| `isBotHasUsername(username: string)`        | Checks if the bot has this username                                                                                                                                     |
| `isSenderChat`                              | Checks if the message sender is a sender chat                                                                                                                           |
| `isSenderChatHasId(id: number)`             | Checks if the sender chat has this ID                                                                                                                                   |
| `isSenderChatHasUsername(username: string)` | Checks if the sender chat has this username                                                                                                                             |
| `isUserFromReply`                           | Checks if the user is the same user who sent the message to which the bot replied. It is useful that only the user who called up the menu can use the callback buttons. |
| `isAdmin`                                   | Checks if the user is an admin                                                                                                                                          |

Feel free to contribute!
