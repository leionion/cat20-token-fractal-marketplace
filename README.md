# Cat20-token-fractal-marketplace

This is the CAT20 Token marketplace on the Fractal Bitcoin Network, where users can list their CAT20 tokens, and buyers can purchase or place orders for CAT20 tokens. Transactions are executed automatically.  

To create a CAT20 Token marketplace using Node.js with TypeScript on the Fractal Bitcoin Network, you'll need to focus on several core components: creating endpoints for listing and purchasing tokens, and handling transactions with the CAT protocol. Here's a structured guide for building this project.
 
### Project Structure 
   - Organize your project with the following structure:
     ```plaintext  
     cat20-token-marketplace/
     ├── src/
     │   ├── server.ts
     │   ├── routes/ 
     │   ├── controllers/ 
     │   ├── models/
     │   └── utils/
     ├── dist/
     ├── package.json
     └── tsconfig.json
     ```
   This structure helps maintain clean separation of concerns, making the codebase easier to manage.

### Core Code Components

1. **Express Server Setup:**
   In `src/server.ts`, set up your Express server:
   ```typescript
   import express from 'express';
   const app = express();
   const port = 3000;

   app.use(express.json());

   app.listen(port, () => {
     console.log(`Server is running on http://localhost:${port}`);
   });
   ```

2. **Market Model:**
  
   Create `src/models/tokenModel.ts` to define the Token schema:

   ```typescript
   import mongoose, { Document, Schema } from 'mongoose';

   interface IToken extends Document {
     name: string;
     symbol: string;
     price: number;
     available: boolean;
   }

   const TokenSchema: Schema = new Schema({
     name: { type: String, required: true },
     symbol: { type: String, required: true },
     price: { type: Number, required: true },
     available: { type: Boolean, default: true },
   });

   export default mongoose.model<IToken>('Token', TokenSchema);
   ```


3. **Routes and Controllers:**
   - In `src/routes/tokenRoutes.ts`, define routes for listing and purchasing tokens:
     ```typescript
     import { Router } from 'express';
     import { listTokens, purchaseToken } from '../controllers/tokenController';

     const router = Router();

     router.get('/tokens', listTokens);
     router.post('/tokens/:id/purchase', purchaseToken);

     export default router;
     ```
   - In `src/controllers/tokenController.ts`, implement the logic for interacting with these endpoints:
     ```typescript
     import { Request, Response } from 'express';

     let tokens: Token[] = [...];

     export const listTokens = (req: Request, res: Response) => {
       res.json(tokens.filter(token => token.available));
     };

     export const purchaseToken = (req: Request, res: Response) => {
       const tokenId = req.params.id;
       const token = tokens.find(t => t.id === tokenId && t.available);
       if (token) {
         token.available = false;
         res.send(`Token ${token.name} purchased successfully.`);
       } else {
         res.status(404).send('Token not found or already sold.');
       }
     };
     ```

4. **Handling Transactions:**
   Implement functions in `src/utils/transactions.ts` to handle blockchain interactions, such as minting, transferring, and executing CAT20 transactions using the tracker service and CLI tool provided by the CAT20 protocol.
   ```typescript
   import { execSync } from 'child_process';

   export const executeTransaction = (args: string) => {
     const command = `cli-exec-command ${args}`;
     return execSync(command).toString();
   };
   ```

### Building and Running the Project

- Use the following commands to build and run your project:
  ```bash
  yarn build
  npm start
  ```
- The `build` script should compile your TypeScript code, and `start` will run the transpiled JavaScript.

### Testing with Postman

- Verify your endpoints by sending HTTP requests using Postman. Set up a new request for listing tokens with the endpoint `GET http://localhost:3000/tokens` and for purchasing with `POST http://localhost:3000/tokens/:id/purchase`.

This setup provides a comprehensive foundation for your CAT20 Token marketplace and handles the core functionalities needed in such an application. For more detailed usage and testing of components like JWT authentication or integrating additional middleware, refer to concepts discussed in additional resources.

I just published essential code part to construct cat20 token marketplace for fractal bitcoin network.

If you have technical support, need development inquiries, please contact me.

### Contact Info

- **Twitter:** [@chain_sats](https://x.com/chain_sats/)
- **Telegram:** [@inscNix](https://t.me/inscNix/)
