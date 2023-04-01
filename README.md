public class Portfolio {
    private double goldHoldings;
    private double currentValue;

    public Portfolio(double goldHoldings, double currentValue) {
        this.goldHoldings = goldHoldings;
        this.currentValue = currentValue;
    }

    public double getGoldHoldings() {
        return goldHoldings;
    }

    public double getCurrentValue() {
        return currentValue;
    }

    public void addGold(double amount) {
        goldHoldings += amount;
        currentValue = goldHoldings * getCurrentGoldPrice();
    }

    public void subtractGold(double amount) {
        goldHoldings -= amount;
        currentValue = goldHoldings * getCurrentGoldPrice();
    }

    public double getCurrentGoldPrice() {
        // Implement method to get current gold price
    }

    public double getProfitOrLoss(double purchasePrice) {
        double currentPrice = getCurrentGoldPrice();
        return (currentPrice - purchasePrice) * goldHoldings;
    }
}
public void buyGold(double amount, double purchasePrice) {
    addGold(amount);
    // Create wallet transaction record with "CREDIT" type
}

public void sellGold(double amount, double sellingPrice) {
    subtractGold(amount);
    // Create wallet transaction record with "DEBIT" type
}
@GET
@Path("/portfolio")
@Produces(MediaType.APPLICATION_JSON)
public Portfolio getPortfolio(@HeaderParam("Authorization") String authHeader) {
    // Implement authentication and authorization logic
    // Retrieve portfolio information for authenticated user
    return portfolio;
}


const goldTransactionSchema = new mongoose.Schema({
  userId: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  entityUser: { type: mongoose.Schema.Types.ObjectId, ref: 'UserEntity' },
  quantity: { type: Number, required: true },
  amount: { type: Number, required: true },
  type: { type: String, enum: ['CREDIT', 'DEBIT'], required: true },
  status: { type: String, enum: ['FAILED', 'SUCCESS', 'WAITING', 'CANCELED', 'PENDING'], required: true },
  runningBalance: {
    wallet: { type: Number, required: true },
    loyaltyPoints: { type: Number, required: true },
    gold: { type: Number, required: true }
  },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
});


app.get('/users/:userId/growth', async (req, res) => {
  const userId = req.params.userId;

  try {
    // Get the user's current balance
    const user = await User.findById(userId);
    const currentBalance = user.runningBalance.gold;

    // Get the user's gold transactions
    const goldTransactions = await GoldTransaction.find({ userId });

    // Calculate the total amount spent and earned on gold transactions
    let totalAmountSpent = 0;
    let totalAmountEarned = 0;

    goldTransactions.forEach(transaction => {
      if (transaction.type === 'CREDIT') {
        totalAmountEarned += transaction.amount;
      } else {
        totalAmountSpent += transaction.amount;
      }
    });

    // Calculate the user's overall growth or loss
    const overallGrowth = currentBalance + totalAmountEarned - totalAmountSpent;

    res.json({ overallGrowth });
  } catch (error) {
    console.error(error);
    res.status(500).json({ message: 'Internal server error' });
  }
});

