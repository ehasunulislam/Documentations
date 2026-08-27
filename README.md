# 🚀 Node-Cron Complete Guide (Beginner to Advanced)

> A complete guide to scheduling tasks in Node.js using **node-cron**.
> From basic setup to advanced production-ready examples.

---

# 📖 What is Cron Job?

A **Cron Job** is a scheduled task that runs automatically at a specific time or interval.

Think of it like an alarm clock for your code.

### Real World Examples

* Send daily emails at 9:00 AM
* Generate reports every midnight
* Delete expired OTPs every 5 minutes
* Backup database every day
* Clear cache every hour
* Send appointment reminders

Instead of manually running these tasks, Cron Jobs do them automatically.

---

# 📦 Installation

Install node-cron:

```bash
npm install node-cron
```

Using Yarn:

```bash
yarn add node-cron
```

Using PNPM:

```bash
pnpm add node-cron
```

---

# ⚡ Basic Usage

```ts
import cron from 'node-cron';

cron.schedule('* * * * *', () => {
  console.log('Running a task every minute');
});
```

### Output

```bash
Running a task every minute
Running a task every minute
Running a task every minute
...
```

---

# 🧠 How Cron Expression Works

A cron expression consists of 5 fields:

```bash
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of Week (0 - 7)
│ │ │ └──── Month (1 - 12)
│ │ └────── Day of Month (1 - 31)
│ └──────── Hour (0 - 23)
└────────── Minute (0 - 59)
```

---

# 🎯 Common Cron Patterns

## Every Minute

```ts
cron.schedule('* * * * *', () => {
  console.log('Every minute');
});
```

---

## Every 5 Minutes

```ts
cron.schedule('*/5 * * * *', () => {
  console.log('Every 5 minutes');
});
```

---

## Every 10 Minutes

```ts
cron.schedule('*/10 * * * *', () => {
  console.log('Every 10 minutes');
});
```

---

## Every Hour

```ts
cron.schedule('0 * * * *', () => {
  console.log('Every hour');
});
```

---

## Every Day at Midnight

```ts
cron.schedule('0 0 * * *', () => {
  console.log('Every midnight');
});
```

---

## Every Day at 9 AM

```ts
cron.schedule('0 9 * * *', () => {
  console.log('Good morning');
});
```

---

## Every Sunday

```ts
cron.schedule('0 0 * * 0', () => {
  console.log('Sunday task');
});
```

---

# 📅 Cron Cheat Sheet

| Schedule              | Expression     |
| --------------------- | -------------- |
| Every minute          | `* * * * *`    |
| Every 5 minutes       | `*/5 * * * *`  |
| Every 10 minutes      | `*/10 * * * *` |
| Every hour            | `0 * * * *`    |
| Every day at midnight | `0 0 * * *`    |
| Every day at 9 AM     | `0 9 * * *`    |
| Every Sunday          | `0 0 * * 0`    |
| Every month           | `0 0 1 * *`    |
| Every year            | `0 0 1 1 *`    |

---

# 🏗 Project Structure

A clean structure for production applications:

```bash
src/
│
├── cron/
│   ├── deleteExpiredOtp.ts
│   ├── sendReminder.ts
│   └── clearCache.ts
│
├── app.ts
└── server.ts
```

---

# 🔥 Example: Delete Expired OTP

```ts
import cron from 'node-cron';

cron.schedule('*/5 * * * *', async () => {
  console.log('Deleting expired OTPs...');
});
```

Runs every 5 minutes.

---

# 🔥 Example: Database Cleanup

```ts
import cron from 'node-cron';

cron.schedule('0 0 * * *', async () => {
  await prisma.user.deleteMany({
    where: {
      isDeleted: true,
    },
  });

  console.log('Cleanup complete');
});
```

Runs every midnight.

---

# 🔥 Example: Send Email

```ts
import cron from 'node-cron';

cron.schedule('0 9 * * *', async () => {
  console.log('Sending daily emails...');
});
```

Runs every day at 9 AM.

---

# 🌏 Timezone Support

Without timezone:

```ts
cron.schedule('0 9 * * *', () => {
  console.log('Running...');
});
```

With timezone:

```ts
cron.schedule(
  '0 9 * * *',
  () => {
    console.log('Running...');
  },
  {
    timezone: 'Asia/Dhaka',
  }
);
```

Recommended for Bangladesh:

```ts
timezone: 'Asia/Dhaka'
```

---

# ⏸ Stop a Cron Job

```ts
const task = cron.schedule('* * * * *', () => {
  console.log('Running...');
});

task.stop();
```

---

# ▶ Restart a Cron Job

```ts
task.start();
```

---

# ❌ Destroy a Cron Job

```ts
task.destroy();
```

After destroy, the task cannot be restarted.

---

# 🛡 Error Handling

Always wrap async code:

```ts
cron.schedule('*/5 * * * *', async () => {
  try {
    console.log('Running task...');
  } catch (error) {
    console.error(error);
  }
});
```

---

# 🚀 Production Best Practices

## 1. Keep Cron Jobs Small

✅ Good

```ts
cron.schedule('* * * * *', async () => {
  await sendReminder();
});
```

❌ Bad

```ts
cron.schedule('* * * * *', async () => {
  // 500 lines of code
});
```

---

## 2. Use Separate Files

```bash
cron/
├── sendReminder.ts
├── otpCleanup.ts
└── databaseBackup.ts
```

---

## 3. Add Logging

```ts
console.log(
  `[CRON] Started at ${new Date().toISOString()}`
);
```

---

## 4. Handle Failures

```ts
try {
  await someTask();
} catch (error) {
  console.error(error);
}
```

---

## 5. Use Environment Variables

```env
CRON_TIME=*/5 * * * *
```

```ts
cron.schedule(process.env.CRON_TIME!, () => {
  console.log('Running...');
});
```

---

# ⚠ Common Mistakes

### Forgetting to Import

```ts
import cron from 'node-cron';
```

---

### Wrong Cron Expression

❌

```ts
cron.schedule('60 * * * *');
```

Minute cannot be 60.

---

### Running Heavy Tasks Every Minute

❌

```ts
cron.schedule('* * * * *', hugeTask);
```

May overload your server.

---

# 🎓 Beginner → Intermediate → Advanced Roadmap

## Beginner

* Install node-cron
* Run task every minute
* Understand cron expressions

---

## Intermediate

* Database cleanup
* Email scheduling
* Logging
* Error handling

---

## Advanced

* Queue systems
* Background workers
* Multi-server cron handling
* Monitoring
* Retry mechanisms

---

# 💡 Interview Questions

### What is a Cron Job?

A Cron Job is a scheduled task that runs automatically at a specified time or interval.

---

### Why use node-cron?

node-cron allows Node.js applications to automate repetitive tasks without manual intervention.

---

### How do you run a task every 5 minutes?

```ts
cron.schedule('*/5 * * * *', () => {});
```

---

### How do you stop a cron task?

```ts
task.stop();
```

---

# 🎯 Quick Summary

✅ Automates repetitive tasks

✅ Uses cron expressions

✅ Supports timezone

✅ Can start, stop, and destroy tasks

✅ Perfect for emails, reminders, cleanup, reports, backups, and scheduled jobs

---

## Resources

* Node-Cron Package: https://www.npmjs.com/package/node-cron
* Cron Expression Generator: https://crontab.guru

---

<div align="center">

### 🚀 Happy Coding

Build automated systems, save time, and let your server do the repetitive work for you.

Made with ❤️ using Node.js & node-cron

</div>
