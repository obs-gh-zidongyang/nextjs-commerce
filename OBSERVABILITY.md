# OpenTelemetry Observability Setup

This Next.js Commerce application has been instrumented with OpenTelemetry for comprehensive observability including distributed tracing, metrics collection, and structured logging.

## Features

- **Distributed Tracing**: Automatic instrumentation of HTTP requests, database queries, and other operations
- **Metrics Collection**: Performance metrics including response times, throughput, and resource usage
- **Structured Logging**: Centralized logging with trace correlation
- **Client-Side Monitoring**: Browser-based instrumentation for user experience tracking
- **Server-Side Monitoring**: Node.js application instrumentation with auto-detection
- **Observe Integration**: Pre-configured for Observe platform with custom headers

## Configuration

### Environment Variables

Configure the following environment variables to customize the observability setup:

#### Server-Side Configuration

```bash
# OTLP endpoint for telemetry data export
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318

# Optional: Bearer token for authentication
OTEL_EXPORTER_OTLP_BEARER_TOKEN=your-token-here
```

#### Client-Side Configuration

```bash
# Client-side OTLP endpoint (must be prefixed with NEXT_PUBLIC_)
NEXT_PUBLIC_OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4318

# Optional: Client-side bearer token
NEXT_PUBLIC_OTEL_EXPORTER_OTLP_BEARER_TOKEN=your-token-here
```

### Default Configuration

If no environment variables are set, the instrumentation defaults to:

- OTLP Endpoint: `http://localhost:4318`
- Service Name: `nextjs-commerce` (server) / `nextjs-commerce-client` (client)
- No authentication

## Architecture

### Server-Side Instrumentation

- **File**: `otel-server.ts`
- **Initialization**: `instrumentation.ts` (Next.js instrumentation hook)
- **SDK**: NodeSDK with auto-instrumentations
- **Exporters**: OTLP HTTP for traces/logs, OTLP Proto for metrics

### Client-Side Instrumentation

- **File**: `otel-client.ts`
- **Initialization**: `components/otel-client-init.tsx`
- **SDK**: WebTracerProvider with manual instrumentations
- **Instrumentations**: Document load, Fetch API, XMLHttpRequest

## Automatic Instrumentation

The setup includes automatic instrumentation for:

### Server-Side

- HTTP/HTTPS requests and responses
- Express.js, Fastify, Koa, and other web frameworks
- Database connections (MySQL, PostgreSQL, MongoDB, etc.)
- Redis operations
- File system operations
- And many more Node.js libraries

### Client-Side

- Page load performance
- Fetch API calls
- XMLHttpRequest calls
- User interactions
- Navigation timing

## Health Checks

The instrumentation includes built-in health monitoring:

- Startup success/failure logging
- SDK initialization status
- Error tracking with full context
- Graceful shutdown handling

## Development vs Production

### Development

- Uses local OTLP collector (localhost:4318)
- Detailed logging for debugging
- No authentication required

### Production

- Configure `OTEL_EXPORTER_OTLP_ENDPOINT` to your observability platform
- Set `OTEL_EXPORTER_OTLP_BEARER_TOKEN` for authentication
- Metrics and traces exported to production collectors

## Observe Platform Integration

This setup is pre-configured for Observe with:

- Custom headers: `x-observe-target-package`
- Proper content types for different telemetry types
- Optimized batch processing

## Troubleshooting

### Common Issues

1. **Build warnings about winston-transport**: This is expected and doesn't affect functionality
2. **Missing environment variables**: The app will use defaults and log warnings
3. **CORS issues in development**: Ensure your OTLP collector allows cross-origin requests

### Debugging

Enable debug logging by setting:

```bash
OTEL_LOG_LEVEL=debug
```

### Disabling Instrumentation

To temporarily disable instrumentation:

```bash
OTEL_SDK_DISABLED=true
```

## Performance Impact

The instrumentation is designed to have minimal performance impact:

- Asynchronous data export
- Efficient batching
- Sampling strategies available
- Graceful degradation on errors

## Customization

To customize the instrumentation:

1. **Service Names**: Edit the `serviceName` constants in `otel-server.ts` and `otel-client.ts`
2. **Sampling**: Add sampling configuration to the SDK initialization
3. **Custom Attributes**: Add resource attributes in the resource configuration
4. **Additional Instrumentations**: Add more instrumentations to the arrays

## Support

For issues related to:

- OpenTelemetry: Check the [OpenTelemetry documentation](https://opentelemetry.io/docs/)
- Next.js integration: See [Next.js instrumentation docs](https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation)
- Observe platform: Contact Observe support
