---
layout: post
title: "This is a simple test"
date: 2014-04-27
---

#Welcome to my simple test!

This is my new blog where I'm going to be testing some things out. I hope that it doesn't suck too much.

```prettyprint
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{

    // Allow for playing of audio (such as the music app) in the background
    NSError *error;
    BOOL success = [[AVAudioSession sharedInstance] setCategory:AVAudioSessionCategoryAmbient error:&error];
    if (!success) {
        //Handle error
        NSLog(@"%@", [error localizedDescription]);
    }
    else {
        // Yay! It worked!
    }

    [CADPageStartup setupCADPage];
    [MagicalRecord setupAutoMigratingCoreDataStack];
    [TSMessage setDefaultNotificationSoundWithName:@"Alert_3" andExtension:@"m4a"];

    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"MainStoryboard" bundle:[NSBundle mainBundle]];
    UINavigationController *navigationController = [storyboard instantiateViewControllerWithIdentifier:@"NavController"];
    self.window.rootViewController = navigationController;
    CGRect navigationBarRect = navigationController.navigationBar.bounds;

    [self setQueue:[NSNotificationQueue defaultQueue]];

    progressBarQueue = dispatch_queue_create("com.apprhythmia.cadpage.progressbarqueue", NULL);

    //Register for APNS notifications
	[[UIApplication sharedApplication] registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];

    [self.window setHidden:FALSE];

    //[FAProgressViewBar setup];
    [FANavigationBar setupWithFrame:navigationBarRect];

    __block NSDictionary *remoteNotification = [launchOptions objectForKey:UIApplicationLaunchOptionsRemoteNotificationKey];
    if (remoteNotification)
    {
        [self setReceivedWhileInactive:YES];
    }
    else
    {
        [self setReceivedWhileInactive:NO];
    }

    //*****************************************************************************
    //******************************START REACHABILITY SETUP***********************
    //*****************************************************************************

    //Check if device has current data connection via the following blocks
    self.reach = [Reachability reachabilityWithHostname:@"www.google.com"];

    // The following block is triggered if the device has a data connection.  
    self.reach.reachableBlock = ^(Reachability*reach)
    {
        NSLog(@"REACHABLE!");
        if	(remoteNotification)
        {
//            [AllVendors ProcessRemoteNotification:remoteNotification withCompletion:^(BOOL finished){
//                if (finished) {
//                    remoteNotification=nil;
//                };
//            }];
        }
    };

    // The following block is triggered if the device does not have a data connection, and lets the user know of the lack of connection via an Alert View.
    self.reach.unreachableBlock = ^(Reachability*reach)
    {
        NSLog(@"UNREACHABLE!");
        dispatch_async(dispatch_get_main_queue(), ^(){
            [PXAlertView showAlertWithTitle:@"Attention!" message:@"You need to have a data or wifi connection to be able to retrieve calls! Please make sure your device is connected to wifi, or has a cell signal."];
        });
    };

    // Start the notifier which will cause the reachability object to retain itself!
    [self.reach startNotifier];

    //*****************************************************************************
    //******************************END REACHABILITY SETUP*************************
    //*****************************************************************************

    self.notificationDictionary = remoteNotification.mutableCopy;

    [self.window makeKeyWindow];

    [self setIsRetina:[self deviceIsRetina]];

    return YES;
}

```
