#EOS本身还没有支持get transaction by id,以及get actions操作(如下操作所示)，eosDataConditionSearch提起支持这两个功能

##cleos -u http://mainnet.genereos.io  get transaction ec6887d35adfefc9d7443189d9802cdf3b9de24e065f887399827c7e3bf3ffdd
返回结果有问题
{
  "id": "ec6887d35adfefc9d7443189d9802cdf3b9de24e065f887399827c7e3bf3ffdd",
  "trx": null,
  "block_time": "2000-01-01T00:00:00.000",
  "block_num": 0,
  "last_irreversible_block": 3045942,
  "traces": []
}

#c#leos -u http://mainnet.genereos.io  get actions helloworldgo
返回的操作也为空
##  seq  when                              contract::action => receiver      trx id...   args
================================================================================================================

##This program provides eos gettransactionbyid and getactions about transfer and transferfrom (contracts Must meet the erc20 standard or subset) functionality
#这个程序为eos提供gettransactionbyid和getactions(仅仅转账相关)功能,所查询合约操作，必须符合erc20标准或者子集

###Install mongodb,create database eos with your username and password
####Should point to your local nodeos node, so that the speed will be much faster, you can keep up with the nodeos block out speed
eos.url= 


###Then config application.properties

###Deploy,and then StartSyncBlock from latest block number，Before the bolck, you need to simply modify the program and start syncing from the first block
curl "http://127.0.0.1:8080/VX/StartSyncBlock"

###5Now,you can try GetTransactionById and GetActions
curl "http://127.0.0.1:8080/VX/GetTransactionById/ec6887d35adfefc9d7443189d9802cdf3b9de24e065f887399827c7e3bf3ffdd"
curl "http://127.0.0.1:8080/VX/GetActions"  -X POST -d '{"symbols":[{"symbolName":"AAA","contractName":"helloworldgo"},{"symbolName":"EOS","contractName":"eosio.token"}],"from":"prizequiz233", "to":"", "page":0, "pageSize":30}' -H "Content-Type: application/json"

###Create index
####You must create an index, otherwise it will be slow，After synchronizing several blocks, create another index
use eos;
db.auth(username, password);
db.blocks.createIndex({"block_num":1})
db.blocks.createIndex({"transactions.trx.id":1})
db.blocks.createIndex({"transactions.trx.transaction.actions.account":1})