# Migrating to SendBird from Pusher Chatkit

With SendBird, we make it easy to quickly perform a bulk migration of your user, channel and message chat data from Pusher’s Chatkit. Pusher is retiring their Chatkit product on April 23, 2020, but the SendBird team is ready to help you to migrate quickly! Tell us more about your chat migration at [support@sendbird.com](support@sendbird.com)

## Migration Steps
### 1. Export your data from Pusher
* Export your data from Pusher. This data should consist of users, channels and messages. 
* [Sign up for SendBird](https://dashboard.sendbird.com/auth/signup) and create your SendBird applications. 
    We recommend creating two applications: one test / development application and one production application. SendBird can perform a dry run of your migration data into your test/development application prior to migration into your production application so you can verify the data import.When choosing a server region for your applications, select the region that is the closest to your end-users.
* Share your data import with SendBird. To get started, contact us at [support@sendbird.com](support@sendbird.com)

### 2. Prepare your app with SendBird SDK
While your data is importing into SendBird, begin development of a new version of your application running with the SendBird SDK. Find our getting started guides and sample apps below.

**JavaScript**
    [Quick Start Guide](https://docs.sendbird.com/javascript)
    [Sample Apps for Web and React Native](https://github.com/sendbird/sendbird-javascript)
    [See the Sample Apps running in action here!](http://sample.sendbird.com)

**Android**
    [Quick Start Guide](https://docs.sendbird.com/android)
    [Sample App](https://github.com/sendbird/sendbird-android)

**iOS**
    [Quick Start Guide](https://docs.sendbird.com/ios)
    [Sample App](https://github.com/sendbird/SendBird-iOS)

## 3. Deploy app code with SendBird SDK
After your data import is complete, it’s time to release the new app with SendBird SDK. If possible, we recommend force-updating your users to the new app version to quickly move all users to chat running with SendBird. 

If it is not possible to immediately force-update users, another approach is to include the SendBird SDK and the Pusher SDK into your application at the same time. This approach is more complex as it requires your application to handle matching chat users, channels and messages in Pusher and SendBird at the same time. We recommend [getting in touch with us](support@sendbird.com) if this is your must-have use case so we can discuss and provide assistance in moving forward.

# Data Mappings
## Users 
Users can chat with each other by participating in open channels and joining group channels and are identified with their own user id. [Learn more about users here](https://docs.sendbird.com/platform/user).
* **User id**: a string with a length of 80 and can be the same as old id that was used in the legacy system. This will be the unique id that can be used to identify the user and we don’t allow duplicate id in an app. 
* **Metadata**: If you want to add custom information such as organization id for the B2B case, you can use this resource. You can filter users based on this value to limit their access to a certain group of users. Find more information on [user metadata here](https://docs.sendbird.com/platform/user_metadata).


## Messages 
SendBird supports text and file messages for migration and maps them to SendBird MESG and FILE message types.
[Learn more about message types here](https://docs.sendbird.com/platform/messages).
* **Custom Type**: You can use this field to subclassify messages. This field can be used for querying messages.
* **Data**: Reserved for adding custom data information used for clients. The data field stores string type data and customers can use JSON or XML to serialize data into the string. 
* **Dedup id**: A unique id from the legacy system. A duplicate check will be performed on this field so it prevents the same data from getting inserted. 
* **Created at**: Instead of the timestamp set at the saving step, you should assign the original value from the legacy system. 
* **File message**: The file message can include the data of the file to upload to the SendBird server in raw binary format or the URL to the file.


## Channels 
SendBird supports [Group Channels](https://docs.sendbird.com/platform/group_channel#3_create_a_channel) designed for chats with user membership. Group Channels can include 1-1 chats or 1-N chats with multiple members, and Group Channels can be public or private. SendBird also supports [Open Channels](https://docs.sendbird.com/platform/open_channel) intended for large, public chats that users participate in during their active chat session. [Learn more about channel types here](https://docs.sendbird.com/platform/channel_type#2_channel_types).
During migration, when the channel is not found, SendBird creates a channel first and then starts adding messages. 
* **Channel URL**: If you pass "channel_url" as the legacy system's channel identifier, then you don't have to save mapping info between legacy channels and SendBird channels. The channel_url is a unique identifier in your app so a duplicate channel URL will not be allowed. 
* **Data**: Reserved for adding custom data information used for clients. The data field stores string type data and customers can use JSON or XML to serialize data into the string. You can also add origin information to mark that it was migrated. 
* **Custom Type**: This value helps subclassify channels. This field can be used for querying channels. Tip- If the organization that your apps support have a hierarchy, give the customer type as higher_organization_name_sub_organization_name ex. macy_sanfrancisco. You can query channels based on custom_type_equal or custom_type_starts_with operator.
