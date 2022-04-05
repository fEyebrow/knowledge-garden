- 通过web3.js lib连接Solana cluster
	- ```
	  const {network, address} = req.body;
	  const url = getNodeURL(network);
	  const connection = new Connection(url, 'confirmed');
	  ```
- 生成新的keypair，然后airdrop新生成的地址
	- ```
	  const keypair = new Keypair();
	  const address = keypair?.publicKey.toString();
	  const hash = await connection.requestAirdrop(publicKey, LAMPORTS_PER_SOL);
	  await connection.confirmTransaction(hash);
	  ```
- 在多个账户间交换token
	- ```
	      const instructions = SystemProgram.transfer({
	        fromPubkey,
	        toPubkey,
	        lamports,
	      });
	      const transaction = new Transaction().add(instructions);
	      
	      const signers = [{publicKey: fromPubkey, secretKey}];
	      const hash = await sendAndConfirmTransaction(
	        connection,
	        transaction,
	        signers,
	      );
	  ```
- deploying and interacting with Solana program
- ```
  // smart contract
  pub fn process_instruction(
      program_id: &Pubkey,
      accounts: &[AccountInfo],
      _instruction_data: &[u8],
  ) -> ProgramResult
  
  // generate new keypair
  solana-keygen new --outfile solana-wallet/keypair.json
  
  // airdrop sol for deploy
  solana airdrop 1 $(solana-keygen pubkey solana-wallet/keypair.json)
  
  // deploy program
  solana program deploy -v --keypair solana-wallet/keypair.json *.so
  
  // get the info of program deployed
  const publicKey = new PublicKey(programId);
  const programInfo = await connection.getAccountInfo(publicKey);
  ```
- ```
  // 通过program id来派生pubkey，program id 将拥有这个key,并给予它往账号写的权限。
  const greetedPubkey = await PublicKey.createWithSeed(
        payer.publicKey,
        GREETING_SEED,
        programId,
      );
  const lamports = await connection.getMinimumBalanceForRentExemption(
        GREETING_SIZE,
      );
  const transaction = new Transaction().add(
        SystemProgram.createAccountWithSeed({
          fromPubkey: payer.publicKey,
          basePubkey: payer.publicKey,
          seed: GREETING_SEED,
          newAccountPubkey: greetedPubkey,
          lamports,
          space: GREETING_SIZE,
          programId,
        }),
      );
  const hash = await sendAndConfirmTransaction(connection, transaction, [
        payer,
      ]);
  
  const greeterPublicKey = new PublicKey(greeter);
  const accountInfo = await connection.getAccountInfo(greeterPublicKey);
  
  // 将二进制数据转化成数据结构，然后显示
  const greeting = borsh.deserialize(
        GreetingSchema,
        GreetingAccount,
        accountInfo.data,
      );
  
  // send data to the program
  const instruction = new TransactionInstruction({
        keys: [{pubkey: greeterPublicKey, isSigner: false, isWritable: true}],
        programId: programKey,
        data: Buffer.alloc(0),
      });
  
      // this your turn to figure out
      // how to create this transaction
      const hash = await sendAndConfirmTransaction(
        connection,
        new Transaction().add(instruction),
        [payerKeypair],
      );
  
  ```