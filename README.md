public class LoginActivity extends AppCompatActivity {

    Button Entrarbtn, Regitrobtn;
    EditText Correotxt, Contratxt;

    private FirebaseAuth Auth = FirebaseAuth.getInstance();
    private FirebaseUser user = Auth.getCurrentUser();

    private static final String TAG = "EmailPassword";


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        Entrarbtn = (Button) findViewById(R.id.Loginbtn);
        Regitrobtn = (Button) findViewById(R.id.Registerbtn);
        Correotxt = (EditText) findViewById(R.id.Emailtxt);
        Contratxt = (EditText) findViewById(R.id.Passwordtxt);

        Entrarbtn.setOnClickListener(new View.OnClickListener() {

            String correo = Correotxt.getText().toString();
            String contra = Contratxt.getText().toString();

            @Override
            public void onClick(View v) {

                Log.d(TAG, "signIn:" + correo);
                if(!validacion()){
                    return;
                }

                Auth.signInWithEmailAndPassword(correo,contra).addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {

                        if(task.isSuccessful()){
                            Log.d(TAG, "signInWithEmail:success");
                            Toast.makeText(LoginActivity.this, "Loggeado como: "+user+".", Toast.LENGTH_LONG).show();
                            FirebaseUser current = Auth.getCurrentUser();
                            updateUI(current);
                        }else{
                            Log.w(TAG, "signInWithEmail:failure", task.getException());
                            updateUI(null);
                        }

                        if(!task.isSuccessful()){
                            Toast.makeText(LoginActivity.this, "Otro msg "+user+".", Toast.LENGTH_LONG).show();
                        }

                    }

                });
            }

        });

        Regitrobtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent i = new Intent(LoginActivity.this,RegisterAcvitivty.class);
                startActivity(i);
            }
        });

    }

    private void updateUI(FirebaseUser user) {

        if(user != null && !user.getUid().isEmpty()){
            Intent i = new Intent(LoginActivity.this,MainActivity.class);
            startActivity(i);
        }else{
            Toast.makeText(LoginActivity.this, "No hay loggeo previo", Toast.LENGTH_LONG).show();
        }

    }

    private boolean validacion() {

        boolean valid = true;

        String mail = Correotxt.getText().toString();
        if(TextUtils.isEmpty(mail)){
            Correotxt.setError("Campo Requerido");
            valid = false;
        }else{
            Correotxt.setError(null);
        }

        String pass = Contratxt.getText().toString();
        if(TextUtils.isEmpty(pass)){
            Contratxt.setError("Campo Requerido");
            valid = false;
        }else{
            Contratxt.setError(null);
        }

        return valid;
    }

}
