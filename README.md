# 🤖 Telegram Giveaway Bot

A full-featured Telegram bot for managing giveaways in your channels.

## ✨ Features

- ➕ **Add & Manage Channels** - Link multiple channels with admin verification (multiple admins can manage the same channel)
- 🎁 **Create Giveaways** - Step-by-step wizard for easy setup
- 🏆 **Winner Selection** - Random or First-X participant selection
- 🎯 **Required Subscriptions** - Ensure participants join specific channels
- 📊 **Dashboard & Analytics** - Track active/expired giveaways and stats
- ⏰ **Automatic Deadline** - Background task checks and ends giveaways
- 💬 **Prize Distribution** - Automatic DM to winners with their prizes

## 📋 Requirements

- Python 3.11+
- MongoDB (running locally or MongoDB Atlas)
- Telegram Bot Token (from [@BotFather](https://t.me/botfather))
- Telegram API credentials (from [my.telegram.org](https://my.telegram.org))

**Tech Stack:**
- Pyrogram - Telegram MTProto API framework
- Motor - Async MongoDB driver for non-blocking database operations
- MongoDB - Document database for storing giveaways and users

## 🚀 Installation

1. **Clone or download this project**

2. **Install dependencies:**
```bash
pip install -r requirements.txt
```

3. **Setup environment variables:**

Copy `.env.example` to `.env` and fill in your credentials:

```bash
cp .env.example .env
```

Then edit `.env` with your values:

```env
BOT_TOKEN=your_bot_token_here
API_ID=your_api_id_here
API_HASH=your_api_hash_here
MONGODB_URI=mongodb://localhost:27017/
DB_NAME=telegram_giveaway_bot
BOT_OWNER_ID=your_telegram_user_id
```

**How to get these values:**
- `BOT_TOKEN`: Message [@BotFather](https://t.me/botfather) on Telegram
- `API_ID` & `API_HASH`: Get from [my.telegram.org/apps](https://my.telegram.org/apps)
- `BOT_OWNER_ID`: Message [@userinfobot](https://t.me/userinfobot) to get your user ID

4. **Start MongoDB:**

Make sure MongoDB is running on your system.

5. **Fix MongoDB index (if upgrading from old version):**

If you're getting duplicate key errors when multiple admins try to add the same channel, run this migration:

```bash
python3 fix_channel_index.py
```

This updates the database index to allow multiple admins to manage the same channel.

6. **Run the bot:**
```bash
python main.py
```

## 📖 Usage

### Add a Channel

1. Click **➕ Add Channel**
2. Send the channel ID or @username
3. The bot verifies admin permissions
4. Channel is linked and ready for giveaways

### Create a Giveaway

1. Click **🎁 Create Giveaway**
2. Follow the wizard:
   - Select channel
   - Enter title
   - Enter description
   - Set duration (e.g., 5m, 1h, 2d)
   - Set winner count
   - Choose winner type (Random / First X)
   - Send prize(s)
   - Add required subscriptions (optional)
   - Confirm and publish

### Prize Format

**Single Prize (same for all winners):**
```
THM-PREMIUM-KEY-9A3X7B2
```

**Multiple Prizes (one per winner):**
```
johndoe:12345
janedoe:pass4321
THM-PREM-KEY-9Q2W
KEY-AAA1-BBB2-CCC3
```

Supported formats:
- `username:password`
- `email@example.com:password`
- `REDEEM-CODE-12345`
- `KEY-XXXX-YYYY-ZZZZ`

### Dashboard

Click **📊 Dashboard** to:
- View active giveaways
- View expired giveaways
- Check analytics (participants, winners, etc.)

## 🗂 Project Structure

```
project/
├── main.py                 # Entry point
├── config.py              # Configuration loader
├── requirements.txt       # Python dependencies
├── database/              # MongoDB operations
│   ├── connection.py
│   ├── channels.py
│   ├── giveaways.py
│   ├── participants.py
│   ├── winners.py
│   └── user_state.py
├── handlers/              # Pyrogram handlers
│   ├── start.py
│   ├── add_channel.py
│   ├── manage_channels.py
│   ├── create_giveaway.py
│   ├── giveaway_callbacks.py
│   ├── dashboard.py
│   └── help_support.py
├── menus/                 # Keyboard layouts
│   └── keyboards.py
├── services/              # Business logic
│   ├── giveaway_post.py
│   ├── winner_selection.py
│   └── deadline_checker.py
└── utils/                 # Utilities
    ├── validators.py
    └── formatters.py
```

## 🔐 Security Notes

- The bot must be an **admin** in every channel where giveaways are hosted
- Required permissions: **Post messages**, **Edit messages**, **Delete messages**
- All channel validation uses `ChatType.CHANNEL` to ensure only channels are used
- Prizes are sent via private DM to winners only
- **Multi-admin support**: Multiple admins can add the same channel if they are admins in that channel
- The bot verifies that users are channel admins before allowing them to add channels

## 🛠 Troubleshooting

**"Failed to link channel"**
- Ensure the bot is an admin in the channel
- Check that the channel ID/username is correct
- Verify you are an admin in the channel

**"E11000 duplicate key error"**
- Run the migration script: `python3 fix_channel_index.py`
- This fixes the database to allow multiple admins per channel

**"Missing permissions"**
- Grant the bot: Post, Edit, Delete messages permissions

**"Giveaway not posting"**
- Verify bot is still an admin with proper permissions
- Check MongoDB connection

## 📞 Support

For issues or questions, refer to the **Help & Support** menu in the bot.

## 📄 License

This project is provided as-is for educational purposes.
