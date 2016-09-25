transaction_vault
===========

A simple example of a vault storing data in a regular table. The passphrase
protecting the vault is stored in table key_vault. This is intended for user_name
within a single transaction. Under the hood this relies on txid_current() function.
Thus all calls should be within the same transaction for the set/get to work correctly.
The API consists of three functions:

 * set_username(user_name, passphrase)
 * get_username()
 * delete_username()


Example usage
-------------

1. install `pgcrypto` extension (provides crypto for the signing etc.)

    CREATE EXTENSION pgcrypto;

2. install `transaction_vault` extension (after `make install`)

    CREATE EXTENSION transaction_vault;

3. generate random signing key and set a passphrase

    INSERT INTO transaction_vault.key_vault
    VALUES (crypt('mypassphrase', gen_salt('bf')));

4. set the context

    SELECT transaction_vault.set_username('tomas', 'mypassphrase');

5. get the username from signed context

    SELECT transaction_vault.get_username();

6. delete the username from the table so we don't bloat our tables

    SELECT transaction_vault.delete_username();
