7051- p0o1
8051- p1o1
9051- p0o2
10051- p1o2
11051- p0o3
12051- p1o3

peer chaincode install -n medium -v 2.0 -p github.com/chaincode/medium/

#p0o1

export CORE_PEER_ADDRESS=peer0.org1.example.com:7051 export CORE_PEER_LOCALMSPID=Org1MSP export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp


#p1o1

export CORE_PEER_ADDRESS=peer1.org1.example.com:8051 export CORE_PEER_LOCALMSPID=Org1MSP export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer1.org1.example.com/tls/ca.crt export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

#p0o2

export CORE_PEER_ADDRESS=peer0.org2.example.com:9051 export CORE_PEER_LOCALMSPID=Org2MSP export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

#p1o2

export CORE_PEER_ADDRESS=peer1.org2.example.com:10051 export CORE_PEER_LOCALMSPID=Org2MSP export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer1.org2.example.com/tls/ca.crt export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

#p0o3

export CORE_PEER_ADDRESS=peer0.org3.example.com:11051 export CORE_PEER_LOCALMSPID=Org3MSP export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.example.com/peers/peer0.org3.example.com/tls/ca.crt export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.example.com/users/Admin@org3.example.com/msp

#p1o3

export CORE_PEER_ADDRESS=peer1.org3.example.com:12051 export CORE_PEER_LOCALMSPID=Org3MSP export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.example.com/peers/peer1.org3.example.com/tls/ca.crt export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org3.example.com/users/Admin@org3.example.com/msp


#Instantiate

export ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

peer chaincode instantiate -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -v 1.0 -c '{"Args":["init"]}' -P "OR('Org1MSP.member','Org2MSP.member')" --collections-config  $GOPATH/src/github.com/chaincode/medium/collections_config.json

peer chaincode instantiate -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -v 2.0 -c '{"Args":["init"]}' -P "OR('Org1MSP.member','Org2MSP.member')" --collections-config  $GOPATH/src/github.com/chaincode/medium/collections_config.json

#Upgrade

peer chaincode upgrade -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -v 1.1 -c '{"Args":["init"]}' -P "OR('Org1MSP.member','Org2MSP.member')" --collections-config  $GOPATH/src/github.com/chaincode/medium/collections_config.json

#Invoke

peer chaincode invoke -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["createProduct","1","log","brown","1m","2m", "100", "200"]}'


peer chaincode invoke -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["createProduct","2","keypad","black","1m","2m", "150", "250"]}'

peer chaincode invoke -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["createProduct","2","keypad","black","1m","2m"]}'

#Query

peer chaincode query -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["getProduct","1"]}'

peer chaincode query -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["getProductPrice","1"]}'

peer chaincode query -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["getProduct","2"]}'

peer chaincode query -o orderer.example.com:7050 --tls --cafile $ORDERER_CA -C mychannel -n medium -c '{"Args":["getProductPrice","2"]}'





