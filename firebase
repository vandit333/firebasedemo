import 'package:firebase_auth/firebase_auth.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
import 'package:firebase_database/firebase_database.dart';
import 'package:google_sign_in/google_sign_in.dart';
import 'package:flutter_facebook_auth/flutter_facebook_auth.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MaterialApp(
    home: firebasedemo(),));
}

class firebasedemo extends StatefulWidget {

  @override
  State<firebasedemo> createState() => _firebasedemoState();
}

class _firebasedemoState extends State<firebasedemo> {

  Future<UserCredential> signInWithGoogle() async {
    // Trigger the authentication flow
    final GoogleSignInAccount? googleUser = await GoogleSignIn().signIn();

    // Obtain the auth details from the request
    final GoogleSignInAuthentication? googleAuth = await googleUser?.authentication;

    // Create a new credential
    final credential = GoogleAuthProvider.credential(
      accessToken: googleAuth?.accessToken,
      idToken: googleAuth?.idToken,
    );

    // Once signed in, return the UserCredential
    return await FirebaseAuth.instance.signInWithCredential(credential);
  }

  Future<UserCredential> signInWithFacebook() async {
    // Trigger the sign-in flow
    final LoginResult loginResult = await FacebookAuth.instance.login();

    // Create a credential from the access token
    final OAuthCredential facebookAuthCredential = FacebookAuthProvider.credential(loginResult.accessToken!.token);

    // Once signed in, return the UserCredential
    return FirebaseAuth.instance.signInWithCredential(facebookAuthCredential);
  }

  TextEditingController t1=TextEditingController();
  TextEditingController t2=TextEditingController();
  TextEditingController t3=TextEditingController();
  TextEditingController t4=TextEditingController();
  String? vid;
  List key=[];
  FirebaseAuth auth = FirebaseAuth.instance;

  @override
  Widget build(BuildContext context) {
    return Scaffold(appBar: AppBar(
      title: Text("Firebase"),
    ),
      body: Column(
        children: [
          TextField(controller: t1,),
          TextField(controller: t2,),
          TextField(controller: t3,),
          TextField(controller: t4,),

          ElevatedButton(onPressed: () async {
            await FirebaseAuth.instance.verifyPhoneNumber(
              phoneNumber: '+91${t1.text}',
              timeout: const Duration(seconds: 120),
              verificationCompleted: (PhoneAuthCredential credential) {

              },
              verificationFailed: (FirebaseAuthException e) {
                print(e.message);
                if (e.code == 'invalid-phone-number') {
                  print('The provided phone number is not valid.');
                }
              },
              codeSent: (String verificationId, int? resendToken) {
                setState(() {
                  vid=verificationId;
                });

              },
              codeAutoRetrievalTimeout: (String verificationId) {

              },
            );
            
          }, child: Text("Send otp")),
          ElevatedButton(onPressed: () async {

            String smsCode='${t2.text}';
            // Create a PhoneAuthCredential with the code
            PhoneAuthCredential credential = PhoneAuthProvider.credential(verificationId: vid!, smsCode: smsCode);

            // Sign the user in (or link) with the credential
            auth.signInWithCredential(credential).then((value) {
              print(value.user!.phoneNumber);
            });
          }, child: Text("verification otp")),
          ElevatedButton(onPressed: () async {
            String name=t3.text;
            String contact=t4.text;
            FirebaseDatabase database = FirebaseDatabase.instance;
            DatabaseReference ref = FirebaseDatabase.instance.ref("student").child("user").push();
            await ref.set({
              "name": name,
              "contact": contact,

            });

          }, child: Text("insert")),
          ElevatedButton(onPressed: () {
            signInWithGoogle();
          }, child: Text("Google")),
          ElevatedButton(onPressed: () {
            signInWithFacebook();
          }, child: Text("Facebook"))
        ],
      ),
    );
  }
}



