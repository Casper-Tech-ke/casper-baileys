# Casper Baileys

A stable, enhanced WhatsApp Web API library forked from Baileys with built-in button support, patched libsignal, and interactive message capabilities.

**By Casper Tech Kenya**

## Features

- **Built-in Button Support** - Send interactive buttons (quick_reply, cta_url, cta_copy, cta_call, single_select) without extra packages
- **Carousel Cards** - Send carousel messages with multiple cards and buttons
- **Binary Node Injection** - Automatic `biz`, `interactive`, and `native_flow` node injection for proper button rendering
- **Patched libsignal** - Suppressed noisy session error logging (Bad MAC, session entry spam) for clean console output
- **Bundled libsignal** - No external libsignal dependency - everything is self-contained
- **Group Status Support** - Send status/stories to groups via GiftedStatus module

## Installation

```bash
npm install github:Casper-Tech-ke/casper-baileys
```

## Usage

### Basic Setup

```javascript
const { default: makeWASocket, useMultiFileAuthState } = require('@whiskeysockets/baileys');

const { state, saveCreds } = await useMultiFileAuthState('./auth');
const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true,
});

sock.ev.on('creds.update', saveCreds);
```

### Sending Interactive Buttons

```javascript
// Quick reply buttons
await sock.sendMessage(jid, {
    text: 'Hello! Choose an option:',
    footer: 'Powered by Casper Baileys',
    interactiveButtons: [
        {
            name: 'quick_reply',
            buttonParamsJson: JSON.stringify({
                display_text: 'Option 1',
                id: 'opt_1'
            })
        },
        {
            name: 'quick_reply',
            buttonParamsJson: JSON.stringify({
                display_text: 'Option 2',
                id: 'opt_2'
            })
        }
    ]
});

// URL button
await sock.sendMessage(jid, {
    text: 'Visit our website!',
    footer: 'Casper Tech',
    interactiveButtons: [
        {
            name: 'cta_url',
            buttonParamsJson: JSON.stringify({
                display_text: 'Visit Website',
                url: 'https://example.com'
            })
        }
    ]
});

// Copy button
await sock.sendMessage(jid, {
    text: 'Copy this code:',
    interactiveButtons: [
        {
            name: 'cta_copy',
            buttonParamsJson: JSON.stringify({
                display_text: 'Copy Code',
                id: 'ABC123',
                copy_code: 'ABC123'
            })
        }
    ]
});
```

### Sending Carousel Cards

```javascript
await sock.sendMessage(jid, {
    text: 'Check out our products!',
    footer: 'Casper Tech Store',
    cards: [
        {
            image: { url: 'https://example.com/product1.jpg' },
            title: 'Product 1',
            body: 'Amazing product',
            buttons: [
                {
                    name: 'quick_reply',
                    buttonParamsJson: JSON.stringify({
                        display_text: 'Buy Now',
                        id: 'buy_1'
                    })
                }
            ]
        },
        {
            image: { url: 'https://example.com/product2.jpg' },
            title: 'Product 2',
            body: 'Great product',
            buttons: [
                {
                    name: 'cta_url',
                    buttonParamsJson: JSON.stringify({
                        display_text: 'View Details',
                        url: 'https://example.com/product2'
                    })
                }
            ]
        }
    ]
});
```

### List/Select Menu

```javascript
await sock.sendMessage(jid, {
    text: 'Select from the menu:',
    footer: 'Casper Baileys',
    interactiveButtons: [
        {
            name: 'single_select',
            buttonParamsJson: JSON.stringify({
                title: 'Menu',
                sections: [
                    {
                        title: 'Options',
                        rows: [
                            {
                                header: 'Option A',
                                title: 'First option',
                                description: 'Description for A',
                                id: 'option_a'
                            },
                            {
                                header: 'Option B',
                                title: 'Second option',
                                description: 'Description for B',
                                id: 'option_b'
                            }
                        ]
                    }
                ]
            })
        }
    ]
});
```

## Button Types

| Type | Description |
|------|-------------|
| `quick_reply` | Simple reply button with text and ID |
| `cta_url` | Opens a URL when tapped |
| `cta_copy` | Copies text to clipboard |
| `cta_call` | Initiates a phone call |
| `single_select` | Dropdown list/menu selection |

## Stability Improvements

- **Patched libsignal** - No more "Bad MAC" or "session entry" console spam
- **Bundled dependencies** - libsignal is included directly, no external GitHub dependency
- **Clean logging** - Suppressed noisy error messages during session renegotiation

## Credits

- Original Baileys by [@whiskeysockets](https://github.com/WhiskeySockets/Baileys)
- Button support inspired by community forks
- Maintained by **Casper Tech Kenya** / **TRABY CASPER**

## License

MIT
