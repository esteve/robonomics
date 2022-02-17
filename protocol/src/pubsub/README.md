## Run:
```
target/debug/robonomics --dev --tmp
```


## Curl:
```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "pubsub_peer"}' http://localhost:9933/
```


## JS client:
```
import { ApiPromise, WsProvider } from '@polkadot/api';

async function main() {
    // The first node port is always 9944. Please check the log for other nodes.
    const wsprovider = new wsprovider('ws://127.0.0.1:9944');
    const api = await apipromise.create({
        provider: wsprovider,
        rpc: {
            pubsub: {
                peer: {
                    description: 'Peer ID',
                    params: [],
                    type: 'string'
                },
                listen: {
                    description: 'Listen',
                    params: [{
                        name: 'address',
                        type: 'string'
                    }],
                    type: 'bool'
                },
                listeners: {
                    description: 'Listeners',
                    params: [],
                    type: 'string'
                },
                connect: {
                    description: 'Connect',
                    params: [{
                        name: 'address',
                        type: 'string'
                    }],
                    type: 'bool'
                },
                subscribe: {
                    description: 'Subscribe',
                    params: [{
                        name: 'topic_name',
                        type: 'string'
                    }],
                    type: '()'
                },
                unsubscribe: {
                    description: 'Unsubscribe',
                    params: [{
                        name: 'topic_name',
                        type: 'string'
                    }],
                    type: 'bool'
                },
                publish: {
                    description: 'Publish',
                    params: [{
                        name: 'topic_name',
                        type: 'string'
                    }, {
                        name: 'message',
                        type: 'string'
                    }],
                    type: 'bool'
                },
            },
        },
    });

    console.log(await api.rpc.pubsub.peer());

    // "/ip4/127.0.0.1/tcp/33333" for the second node
    console.log(await api.rpc.pubsub.listen("/ip4/127.0.0.1/tcp/44444"));

    console.log(await api.rpc.pubsub.listeners());

    console.log(await api.rpc.pubsub.connect("/ip4/127.0.0.1/tcp/33333"));

    const unsubscribe = await api.rpc.pubsub.subscribe("topic_name", ({c}) => {
        console.log(`${c}`);
    });
    setTimeout(() => {
        unsubscribe();
        console.log('unsubscribed');
    }, 8000);

    console.log(await api.rpc.pubsub.publish("topic_name", "test1"));
    console.log(await api.rpc.pubsub.publish("topic_name", "test2"));
}

main().catch(console.error);
```
