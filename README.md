# Machine learning model using python to predict crypto currency prices and based on the model built trading strategy to maximize trading gains.

# Melbourne_Datathon
My submission in Melbourne Datathon
# Scenario

You are a trader for a pension fund that makes money for it's investors by constantly buying and selling financial instruments such as shares in companies, foreign currencies or futures contracts. The general idea is that you aim to buy when the price is low and sell when it is high, hence making a profit.

Traditionally you'd stare at the live prices on a screen and through your experience know exactly when to trade. Recently your company has employed a team of machine learning experts to predict if prices are going up or down so as to hopefully remove the guess work form your trading decisions.

Every 5 minutes you are now receiving 14 different predictions from mathematical models. What they forgot to tell you is how to actually use those predictions. 

The challenge is to figure out how to use these predictions to determine trading decisions. The winner of the challenge will be the 'trader' who's 'strategy' makes the most money.

# The Data

You have been provided with over 12 months of actual prices and predictions of future price movements at 5 minute intervals for some crypto-currency pairs. This is the data you will use to develop your 'trading strategy'.

Additionally you have also been provided with just the predictions (no prices) for some further currency pairs over the same time period. This is the data on which you will apply your strategy to come up with some 'buy' and 'sell' times that determine a 'trade'.

These times will be submitted and the returns calculated.

# What does a 'strategy' look like?

A trading strategy uses only the predictions to make 'entry' and 'exit' trading decisions. It can be very simple logic or sophisticated logic - it is the challenge to come up with that logic. 

e.g
If prediction4 > 0.4 & prediction7 > 0.2  then 'Enter Long' (buy)
If prediction4 < 0.3 & prediction7 < 0.2 then 'Exit Long' (sell)

# What does a Submission look like?

You will be submitting a file with 6 columns. Each row of the file will be a trade that will have it's return calculated.
keys_pair:   the pair to trade 
enterTime:  the time to enter the trade
exitTime:     the time to exit the trade
direction:     are we going long (1) or short (0)  
percentPair : what percent of the 'pair balance' do we want to put on this trade
strategyName: what are you calling this strategy (for reference)

The times we supply are just integers which represent 'minutes from start', hence no need to mess around with time stamps.

Direction means are we predicting the price will go up ('going long') or down ('short selling').

percentPair is the percent of the pair 'balance' that we want to put on each trade. There can be multiple trades for a single pair that overlap in time, but the sum of percentPair for that time stamp cannot be more than 1 (for each direction - see below*). 


# Long and Short?

It's possible to still make money if we think the price is going down, which is called 'short selling'. This is how it's done,

Say X are 10 shares in IBM and we think the share price is dropping.

Rather than buy X, we borrow X from a stockbroker and immediately sell it on the stock exchange. When the price drops we buy X back from the exchange and return X to the stockbroker. We have made a profit as we sold X for more than we bought it back. We would pay the stockbroker 'interest' out of our profit.

'going long' is simpler as we expect the price to go up, so the stockbroker does not need to be involved. We simple 'buy low and sell high' directly from the stock exchange.

*For this exercise, Long and Short trade directions are evaluated separately, which means you will get a total return for long trades and a total return for short trades. Each submitted trades file can contain just longs, just shorts, or both longs and shorts. 



# How is the Strategy Evaluated?

The general idea of how the strategies are evaluated are it is just a sum of the individual percentage gains of each trade. There is no re-investment of any profit.

    for each trade we determine the enter (EN) and exit (EX) prices
     
    the enter and exit prices are adjusted for the trading fee percentage (F), and then the trade percentage gain (TPG) calculated as a percentage of the entry price

    long: ( (EX*(1-F)) - (EN*(1+F)) )/ EN

    short: ( (EN*(1-F)) - (EX*(1+F)) )/ EN
     
    The percent weighting is applied
    TPG = TPG * percentPair
     
    TPG is capped
     
    It is determined if a stoploss has occurred
     
     TPG  then summed over all trades to give a total return



# Fees?

There is a cost associated with each trade made, called the fee. This means that we don't quite get the actual price price as the exchange takes their cut. When we want to buy, we pay slightly more, and when we want to sell we get slightly less. The fee is applied to the price on entering and exiting a trade.

Hence when we make a trade, we need to be confident the returns will cover the fees we are charged. For this exercise, the fee F = 0.15%.

# Capping?

We need to have confidence that the return of each strategy is not influenced by one or two 'lucky' large trades. To do this we take all raw TPGs and calculate the mean (M) and standard deviation (SD) of these individual gains. Any trade that has a return greater than M + (2*SD) or less than M - (2*SD) is capped at that value. This has the effect of removing extreme values. 

# Stop Losses?

If the price goes too far in the wrong direction then a 'stop loss' will be executed for that trade. For example, if we are 'going long' (hoping the price will go up) but the price falls by more than X% before the trade exit time is reached, then the trade will be immediately exited for you. This trade will use the price at the time the stop loss was triggered to calculate the trade returns.

For this exercise the stop loss value is 5%

# Are there any other trade restrictions?

trade length

A 'dumb' strategy would just be to do a single trade for each currency pair that lasts the whole of the time period and guess if you think think it should be a long (price going up) or a short (price going down). Such a strategy might end up making a profit but it would be just guessing and not using the predictions. To prevent this, any trades that are longer than 4 days (60 x 24 x 4 minutes) in length will be eliminated.

# number of trades

It's also possible if you think the predictions are no good to just submit zero trades and have a return of 0, hoping everyone else's strategy loses money. Hence all currency pairs must have at least 5 trades each.
