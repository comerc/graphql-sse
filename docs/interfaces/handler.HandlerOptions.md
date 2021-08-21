[graphql-sse](../README.md) / [handler](../modules/handler.md) / HandlerOptions

# Interface: HandlerOptions

[handler](../modules/handler.md).HandlerOptions

## Table of contents

### Properties

- [context](handler.HandlerOptions.md#context)
- [schema](handler.HandlerOptions.md#schema)

### Methods

- [authenticate](handler.HandlerOptions.md#authenticate)
- [execute](handler.HandlerOptions.md#execute)
- [onComplete](handler.HandlerOptions.md#oncomplete)
- [onConnected](handler.HandlerOptions.md#onconnected)
- [onConnecting](handler.HandlerOptions.md#onconnecting)
- [onDisconnect](handler.HandlerOptions.md#ondisconnect)
- [onNext](handler.HandlerOptions.md#onnext)
- [onOperation](handler.HandlerOptions.md#onoperation)
- [onSubscribe](handler.HandlerOptions.md#onsubscribe)
- [subscribe](handler.HandlerOptions.md#subscribe)
- [validate](handler.HandlerOptions.md#validate)

## Properties

### context

• `Optional` **context**: [`ExecutionContext`](../modules/handler.md#executioncontext) \| (`req`: `IncomingMessage`, `args`: `ExecutionArgs`) => [`ExecutionContext`](../modules/handler.md#executioncontext) \| `Promise`<[`ExecutionContext`](../modules/handler.md#executioncontext)\>

A value which is provided to every resolver and holds
important contextual information like the currently
logged in user, or access to a database.

Note that the context function is invoked on each operation only once.
Meaning, for subscriptions, only at the point of initialising the subscription;
not on every subscription event emission. Read more about the context lifecycle
in subscriptions here: https://github.com/graphql/graphql-js/issues/894.

___

### schema

• `Optional` **schema**: `GraphQLSchema` \| (`req`: `IncomingMessage`, `args`: `Omit`<`ExecutionArgs`, ``"schema"``\>) => `GraphQLSchema` \| `Promise`<`GraphQLSchema`\>

The GraphQL schema on which the operations will
be executed and validated against.

If a function is provided, it will be called on every
subscription request allowing you to manipulate schema
dynamically.

If the schema is left undefined, you're trusted to
provide one in the returned `ExecutionArgs` from the
`onSubscribe` callback.

## Methods

### authenticate

▸ `Optional` **authenticate**(`req`, `res`): `undefined` \| `string` \| `void` \| `Promise`<`undefined` \| `string` \| `void`\>

Authenticate the client. Returning a string indicates that the client
is authenticated and the request is ready to be processed.

A token of type string MUST be supplied; if there is no token, you may
return an empty string (`''`);

If you want to respond to the client with a custom status or body,
you should do so using the provided `res` argument which will stop
further execution.

**`default`** 'req.headers["x-graphql-stream-token"] || req.url.searchParams["token"] || generateRandomUUID()' // https://gist.github.com/jed/982883

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |
| `res` | `ServerResponse` |

#### Returns

`undefined` \| `string` \| `void` \| `Promise`<`undefined` \| `string` \| `void`\>

___

### execute

▸ `Optional` **execute**(`args`): [`OperationResult`](../modules/handler.md#operationresult)

Is the `execute` function from GraphQL which is
used to execute the query and mutation operations.

#### Parameters

| Name | Type |
| :------ | :------ |
| `args` | `ExecutionArgs` |

#### Returns

[`OperationResult`](../modules/handler.md#operationresult)

___

### onComplete

▸ `Optional` **onComplete**(`req`, `args`): `void` \| `Promise`<`void`\>

The complete callback is executed after the operation
has completed and the client has been notified.

Since the library makes sure to complete streaming
operations even after an abrupt closure, this callback
will always be called.

First argument, the request, is always the GraphQL operation
request.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |
| `args` | `ExecutionArgs` |

#### Returns

`void` \| `Promise`<`void`\>

___

### onConnected

▸ `Optional` **onConnected**(`req`): `void` \| `Promise`<`void`\>

Called when a new event stream has been succesfully connected and
accepted, and after all pending messages have been flushed.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |

#### Returns

`void` \| `Promise`<`void`\>

___

### onConnecting

▸ `Optional` **onConnecting**(`req`, `res`): `void` \| `Promise`<`void`\>

Called when a new event stream is connecting BEFORE it is accepted.
By accepted, its meant the server responded with a 200 (OK), alongside
flushing the necessary event stream headers.

If you want to respond to the client with a custom status or body,
you should do so using the provided `res` argument which will stop
further execution.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |
| `res` | `ServerResponse` |

#### Returns

`void` \| `Promise`<`void`\>

___

### onDisconnect

▸ `Optional` **onDisconnect**(`req`): `void` \| `Promise`<`void`\>

Called when an event stream has disconnected right before the
accepting the stream.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |

#### Returns

`void` \| `Promise`<`void`\>

___

### onNext

▸ `Optional` **onNext**(`req`, `args`, `result`): `void` \| `ExecutionResult`<`Object`, `Object`\> \| `Promise`<`void` \| `ExecutionResult`<`Object`, `Object`\>\>

Executed after an operation has emitted a result right before
that result has been sent to the client.

Results from both single value and streaming operations will
invoke this callback.

Use this callback if you want to format the execution result
before it reaches the client.

First argument, the request, is always the GraphQL operation
request.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |
| `args` | `ExecutionArgs` |
| `result` | `ExecutionResult`<`Object`, `Object`\> |

#### Returns

`void` \| `ExecutionResult`<`Object`, `Object`\> \| `Promise`<`void` \| `ExecutionResult`<`Object`, `Object`\>\>

___

### onOperation

▸ `Optional` **onOperation**(`req`, `res`, `args`, `result`): `void` \| [`OperationResult`](../modules/handler.md#operationresult) \| `Promise`<`void` \| [`OperationResult`](../modules/handler.md#operationresult)\>

Executed after the operation call resolves. For streaming
operations, triggering this callback does not necessarely
mean that there is already a result available - it means
that the subscription process for the stream has resolved
and that the client is now subscribed.

The `OperationResult` argument is the result of operation
execution. It can be an iterator or already a value.

Use this callback to listen for GraphQL operations and
execution result manipulation.

If you want to respond to the client with a custom status or body,
you should do so using the provided `res` argument which will stop
further execution.

First argument, the request, is always the GraphQL operation
request.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |
| `res` | `ServerResponse` |
| `args` | `ExecutionArgs` |
| `result` | [`OperationResult`](../modules/handler.md#operationresult) |

#### Returns

`void` \| [`OperationResult`](../modules/handler.md#operationresult) \| `Promise`<`void` \| [`OperationResult`](../modules/handler.md#operationresult)\>

___

### onSubscribe

▸ `Optional` **onSubscribe**(`req`, `res`, `params`): `void` \| `ExecutionArgs` \| `Promise`<`void` \| `ExecutionArgs`\>

The subscribe callback executed right after processing the request
before proceeding with the GraphQL operation execution.

If you return `ExecutionArgs` from the callback, it will be used instead of
trying to build one internally. In this case, you are responsible for providing
a ready set of arguments which will be directly plugged in the operation execution.

Omitting the fields `contextValue` from the returned `ExecutionArgs` will use the
provided `context` option, if available.

If you want to respond to the client with a custom status or body,
you should do so using the provided `res` argument which will stop
further execution.

Useful for preparing the execution arguments following a custom logic. A typical
use-case is persisted queries. You can identify the query from the request parameters
and supply the appropriate GraphQL operation execution arguments.

#### Parameters

| Name | Type |
| :------ | :------ |
| `req` | `IncomingMessage` |
| `res` | `ServerResponse` |
| `params` | [`RequestParams`](common.RequestParams.md) |

#### Returns

`void` \| `ExecutionArgs` \| `Promise`<`void` \| `ExecutionArgs`\>

___

### subscribe

▸ `Optional` **subscribe**(`args`): [`OperationResult`](../modules/handler.md#operationresult)

Is the `subscribe` function from GraphQL which is
used to execute the subscription operation.

#### Parameters

| Name | Type |
| :------ | :------ |
| `args` | `ExecutionArgs` |

#### Returns

[`OperationResult`](../modules/handler.md#operationresult)

___

### validate

▸ `Optional` **validate**(`schema`, `documentAST`, `rules?`, `typeInfo?`, `options?`): readonly `GraphQLError`[]

A custom GraphQL validate function allowing you to apply your
own validation rules.

#### Parameters

| Name | Type |
| :------ | :------ |
| `schema` | `GraphQLSchema` |
| `documentAST` | `DocumentNode` |
| `rules?` | readonly `ValidationRule`[] |
| `typeInfo?` | `TypeInfo` |
| `options?` | `Object` |
| `options.maxErrors?` | `number` |

#### Returns

readonly `GraphQLError`[]