 There are two types of user privileges that a user can have:
 
* Save Card level - lets user save cards, get the saved cards, payment using Credit and Debit cards and delete cards.

* Citrus Cash level - includes all the privileges from Save Card level in addition to that User can now load and pay using their Citrus Cash account.


####Bind User (for only Save Card level access)
bind the user, this grants cards saving, fetching, payment using CC and DC and delete cards privileges to the user, doesn't need password. call this method every time you want to initiate Citrus Saved card and related functionalities.

######note : if you are going to integrate Citrus Cash feature then you don't need to do Bind User you can directly refer to Link User. with Link User you will get Save Card level access too

     [authLayer requestBindUsername:@"test@gmail.com" mobile:@"9702222222" completionHandler:^(NSString *userName, NSError *error) {
              if(error == nil){
                 // Your code to handle success.
             }
             else {
                 // Your code to handle error.
             }
         }];



####Link User (for Citrus Cash level access, includes Save Card level)

Citrus Link user and Sign-in api simplifies the process of new user sign-up as well as enquiring the status of user's existing Citrus Cash account.

Acquiring the Citrus Cash access for the user is a two step process:
First Linking the user and then presenting the sign-in screen with either OTP or password or both depending on the response of the Link User API 


#####Citrus Link User
When User decides to Pay using Citrus Wallet, First thing you need to check is if user is already logged in with api `[authLayer isLoggedIn]`, if they are already logged in you can start the prepaid activities, if they are not then you need to call Citrus Link Api, Link User does all the checks on users account and responds with status that helps you to build sign-in screen. it also creates new account for new users and may fire otp to mobile or email . 

    [authLayer requestCitrusLink:@“test@mailinator.com” mobile:@“9700000000” completion:^(CTSCitrusLinkRes *linkResponse, NSError *error) {
            if (error) {
            [UIUtility toastMessageOnScreen:[error localizedDescription]];
            }
        else{
            [UIUtility toastMessageOnScreen:linkResponse.userMessage];
            
            switch (linkResponse.siginType) {
                case CitrusSiginTypeMOtpOrPassword:
                    // Show Mobile otp and password sign in screen
                    break;
                case CitrusSiginTypeMOtp:
                    // Show Mobile otp sign in screen
                    break;
                case CitrusSiginTypeEOtpOrPassword:
                    // Show Email otp and password sign in screen
                    break;
                case CitrusSiginTypeEOtp:
                    // Show Email otp sign in screen
                    break;
                default:
                    break;
            }
         }
    }];

Citrus Link User responds with CTSCitrusLinkRes object
To decide whether to show Password or OTP or both in the next login screen can be decided using the enum field “linkResponse.siginType” in the link user response. Please see the code above .


#####Citrus Link Sign-in
To Signin with Password or OTP use following API

######Password Sign-in
    [authLayer requestCitrusLinkSignInWithPassoword:@“CitrusPassword” passwordType:PasswordTypePassword completionHandler:^(NSError *error) {
            LogTrace(@"error %@",error);
            if (error) {
                // Handle Error
            }
            else{
                // Handle Success 
            }
        }];


######OTP Sign-in
    [authLayer requestCitrusLinkSignInWithPassoword:@“3453” passwordType:PasswordTypeOtp completionHandler:^(NSError *error) {
            LogTrace(@"error %@",error);
            if (error) {
                // Handle Error
            }
            else{
                // Handle Success 
            }
        }];


####See if anyone is logged in
To check if user is already logged into the SDK.

    if([authLayer isLoggedIn]){
        [UIUtility toastMessageOnScreen:@"user is signed in"];
    }
    else{
        [UIUtility toastMessageOnScreen:@"no one is logged in"];
    }



####Reset User Password
It happens to all of us, we tend to forget our passwords. so if user forgets their password and wants to reset it following is the way to do it 

     [authLayer requestResetPassword:@"test@gmail.com" completionHandler:^(NSError *error) {
         if(error == nil){
                 // Your code to handle success.
             }
             else {
                 // Your code to handle error.
             }
       }];


####Sign out
To do a local sign out from the SDK you have to call following method 
   
      [authLayer signOut];
