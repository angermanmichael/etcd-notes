
the **message** and **msgapp** {encoder, decoder} pairs are both referenced
in stream.

**startStreamReader** and **startStreamWriter** both get fired off in **startPeer**.

**startPeer** gets fired off in the **AddPeer** method located in transport.go.

The **AddPeer** method is called throughout *server.go*
