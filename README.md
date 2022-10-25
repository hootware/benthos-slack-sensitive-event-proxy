# benthos-slack-sensitive-event-proxy

This repo contains a [Benthos](https://benthos.dev) config that ensures downstream services do not receive any Slack message text.

## Background

The Slack API currently does not have a way to only listen for user mentions, therefore all messages in a channel need to be received, at which point user mentions can be extracted.

## Security

This config ensures that the HTTP input terminates SSL requests meaning that intermediate services or reverse proxies will need to pass through the encrypted HTTP request through to the benthos instance.

## Removing sensitive message data

Messages can contain sensitive information. If the event is a message then user mentions are extracted. The original message text and blocks are then removed before forwarding on, therefore removing the sensitive message text. All of this means that the original message text is received, stored in memory for a few milliseconds and deleted. Therefore, no message data is ever stored or persisted.

## Other event types

All event types that are not messages are forwarded on to the downstream service without being changed

## Exceptions

In order for bots to function correctly and respond to messages, any messages sent in bot channels, such as a DM with a bot or a message in an App Home, message text is not removed.
