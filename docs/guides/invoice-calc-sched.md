# Scheduling your invoice calculations using bluectl

!!! note
    This guide is only applicable to Ripple users.

In this guide, we will try and create an invoicing calculation schedule for Ripple every 3rd day of each month. At the moment, only one schedule is allowed per Ripple account.

Make sure to install [`bluectl`](https://alphauslabs.github.io/docs/blueapi/bluectl/) first.

Note that creating a calculation schedule automatically enables your notification channels as well. If you don't specify a notification channel during schedule creation, an email-type channel will be created using your primary email address. At the moment, only email-type channels are supported.

Schedules are specified using the Unix [cron](https://man7.org/linux/man-pages/man5/crontab.5.html) format. For example, every 7th day of the month at 9:00am: `0 9 7 * *`. To create our schedule for this guide (every 3rd of the month), run the following command:

``` sh
$ bluectl cost aws calculation schedule create "0 0 3 * *"
```

Run the following command to see your current schedule:

``` sh
$ bluectl cost aws calculation schedule list
ID        SCHEDULE   NEXT_RUN              NOTIFICATION_CHANNEL
Csqsj4v2  0 0 3 * *  2022-03-03T00:00:00Z  7143c90d-6f84-4051...
```

!!! info "--notification-channel"
    If you want to use an [existing notification channel](https://app.alphaus.cloud/ripple/notification-setting), use the `--notification-channel=id` flag. You can get your channel ids using the following command:

    ``` sh
    $ bluectl notification channels list
    ```

Finally, run the following command to delete your schedule:

``` sh
$ bluectl cost aws calculation schedule rm -
```
