# ================================
# n8n - Workflow Automation Engine
# ================================
FROM n8nio/n8n:latest

# Switch to root for any system-level setup
USER root

# Install additional packages if needed (e.g. for custom nodes)
# RUN apk add --no-cache git

# Switch back to the default n8n user
USER node

EXPOSE 5678

CMD ["n8n", "start"]
