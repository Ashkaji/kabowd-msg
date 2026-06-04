# ================================
# WAHA - WhatsApp HTTP API (NOWEB)
# ================================
# devlikeapro/waha-noweb is the lightweight image for the NOWEB engine
# (no Chromium, no puppeteer — ideal for Render Starter plan)
FROM devlikeapro/waha-noweb:latest

# Create directories required by env vars:
#   WAHA_LOCAL_STORE_BASE_DIR  → sessions & auth tokens
#   WHATSAPP_FILES_FOLDER      → incoming/outgoing media files
RUN mkdir -p /var/lib/waha/storage \
    && mkdir -p /app/whatsapp_files

EXPOSE 3000

CMD ["/app/main"]
