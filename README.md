# individual-project
__author__ = 'yuki'
#Base code
def recMC(coinValueList,change):
   minCoins = change
   if change in coinValueList:# if the change is the same amount of our coins in base case,
     return 1 # it will simply return one
   else:
      for i in [c for c in coinValueList if c <= change]:# filter  coins which less than the amount of the change
         numcoins = 1 + recMC(coinValueList,change-i)#recursive call
         if numcoins < minCoins:
            minCoins = numcoins
   return minCoins

print(recMC([1,5,10,25],63))


#Improved code1
def recDC(coinValueList,change,knownResults):
   minCoins = change
   if change in coinValueList:
      knownResults[change] = 1# if the change is in the valuelist, So the knownresults is this coin and simply return 1.
      return 1
   elif knownResults[change] > 0:# Testing whether the solution for certain amount of change already existed
      return knownResults[change]# If there is already have the result, it will simply return the knownResult
   else:
       for i in [c for c in coinValueList if c <= change]:# If it's not existed in the system, it will compute it and then restore it.
         numCoins = 1 + recDC(coinValueList, change-i,# The calculation process is same as the base code
                              knownResults)
         if numCoins < minCoins:
            minCoins = numCoins
            knownResults[change] = minCoins
   return minCoins

print(recDC([1,5,10,25],63,[0]*64))# 64 is range of the amount, and the change should be less than it.




#Improved code2
def recDC(coinValueList,change,knownResults):
   minCoins = change
   if change in coinValueList:
      knownResults[change] = 1# if the change is in the valuelist, So the knownresults is this coin and simply return 1.
      return 1
   elif knownResults[change] > 0:# Testing whether the solution for certain amount of change already existed
      return knownResults[change]# If there is already have the result, it will simply return the knownResult
   else:
       for i in [c for c in coinValueList if c <= change]:# If it's not existed in the system, it will compute it and then restore it.
         numCoins = 1 + recDC(coinValueList, change-i,# The calculation process is same as the base code
                              knownResults)
         if numCoins < minCoins:
            minCoins = numCoins
            knownResults[change] = minCoins
   return minCoins


def dpMakeChange(coinValueList,change,minCoins):# The minCoins was the answer before, and was restored.
                                                # so reduce the ieration work.
   for cents in range(change+1):# in order to be convinent, the range is the amount of change plus 1,
   # but actually it doesn't matter as long as you change it correspondingly in the last print line.
      coinCount = cents
      for j in [c for c in coinValueList if c <= cents]:# The following calculation process is also similar as before.
            if minCoins[cents-j] + 1 < coinCount:
               coinCount = minCoins[cents-j]+1
      minCoins[cents] = coinCount
   return minCoins[change]
print(recDC([1,5,10,25],63,[0]*64))



#Final code
def dpMakeChange(coinValueList,change,minCoins,coinsUsed):
   for cents in range(change+1):
      coinCount = cents
      newCoin = 1
      for j in [c for c in coinValueList if c <= cents]:
            if minCoins[cents-j] + 1 < coinCount:
               coinCount = minCoins[cents-j]+1
               newCoin = j
      minCoins[cents] = coinCount
      coinsUsed[cents] = newCoin# Since j has already be computed, so it was stored in coinUsed
   return minCoins[change]

def printCoins(coinsUsed,change):
   coin = change
   while coin > 0:
      thisCoin = coinsUsed[coin]# if it was used, it will simply return the coinused
      print(thisCoin)
      coin = coin - thisCoin# And the coin will be the coin minus coinused, than test it again until coin=0.

def main():# List of the value needed.
    amnt = 63
    clist = [1,5,10,21,25]
    coinsUsed = [0]*(amnt+1)
    coinCount = [0]*(amnt+1)

    print("Making change for",amnt,"requires")
    print(dpMakeChange(clist,amnt,coinCount,coinsUsed),"coins")
    print("They are:")
    printCoins(coinsUsed,amnt)
    print("The used list is as follows:")
    print(coinsUsed)

main()



