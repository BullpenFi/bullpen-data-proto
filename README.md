# bullpen-data-proto

Generated gRPC client libraries for Bullpen Data services.

## Languages

| Language | Path | Package |
|----------|------|---------|
| TypeScript | `gen/ts/` | ConnectRPC + protobuf-es v1 |
| Python | `gen/python/` | gRPC + protobuf |
| Go | `gen/go/` | gRPC + protobuf |

## Proto Source

Proto definitions live in `proto/bullpen/v1/`:

| Proto | Service | Description |
|-------|---------|-------------|
| `common.proto` | — | Shared types (pagination, enums, TraderTag) |
| `health.proto` | HealthService | Health checks |
| `polymarket_explore.proto` | PolymarketExploreService | Market discovery, page views, lenses |
| `polymarket_account.proto` | PolymarketAccountService | Profiles, portfolios, API keys |
| `polymarket_trade_info.proto` | PolymarketTradeInfoService | Orderbooks, spreads, holders |
| `polymarket_trade.proto` | PolymarketTradeService | Order execution |

## Usage

### TypeScript (browser / Node.js)

```bash
npm install @bufbuild/protobuf@1 @connectrpc/connect@1 @connectrpc/connect-web@1
```

```typescript
import { createClient } from "@connectrpc/connect";
import { createGrpcWebTransport } from "@connectrpc/connect-web";
import { PolymarketExploreService } from "bullpen-data-proto/gen/ts/bullpen/v1/polymarket_explore_connect";

const transport = createGrpcWebTransport({ baseUrl: "http://localhost:50051" });
const client = createClient(PolymarketExploreService, transport);

const page = await client.getPageView({ pageId: "weather" });
```

### Python

```python
import grpc
from bullpen.v1 import polymarket_explore_pb2_grpc, polymarket_explore_pb2

channel = grpc.insecure_channel("localhost:50051")
stub = polymarket_explore_pb2_grpc.PolymarketExploreServiceStub(channel)

response = stub.GetPageView(
    polymarket_explore_pb2.GetPageViewRequest(page_id="weather")
)
```

### Go

```go
import pb "github.com/BullpenFi/bullpen-data/gen/go/bullpen/v1"

conn, _ := grpc.Dial("localhost:50051", grpc.WithInsecure())
client := pb.NewPolymarketExploreServiceClient(conn)

resp, _ := client.GetPageView(ctx, &pb.GetPageViewRequest{PageId: "weather"})
```

## Regenerating

Requires [buf](https://buf.build/docs/installation):

```bash
buf generate
```

## Lint & Breaking Change Detection

```bash
buf lint
buf breaking --against '.git#branch=main'
```
