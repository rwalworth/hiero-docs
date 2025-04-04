# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/response_code.proto](#services_response_code-proto)
    - [ResponseCodeEnum](#proto-ResponseCodeEnum)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_response_code-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/response_code.proto
# Response Code Enumeration
An enumeration of possible response codes.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).

 


<a name="proto-ResponseCodeEnum"></a>

### ResponseCodeEnum
An enumeration of possible response codes.

| Name | Number | Description |
| ---- | ------ | ----------- |
| OK | 0 | The transaction passed the precheck validations. |
| INVALID_TRANSACTION | 1 | For any error not handled by specific error codes listed below. |
| PAYER_ACCOUNT_NOT_FOUND | 2 | Payer account does not exist. |
| INVALID_NODE_ACCOUNT | 3 | Node Account provided does not match the node account of the node the transaction was submitted to. |
| TRANSACTION_EXPIRED | 4 | Pre-Check error when TransactionValidStart &#43; transactionValidDuration is less than current consensus time. |
| INVALID_TRANSACTION_START | 5 | Transaction start time is greater than current consensus time |
| INVALID_TRANSACTION_DURATION | 6 | The given transactionValidDuration was either non-positive, or greater than the maximum valid duration of 180 secs. |
| INVALID_SIGNATURE | 7 | The transaction signature is not valid |
| MEMO_TOO_LONG | 8 | Transaction memo size exceeded 100 bytes |
| INSUFFICIENT_TX_FEE | 9 | The fee provided in the transaction is insufficient for this type of transaction |
| INSUFFICIENT_PAYER_BALANCE | 10 | The payer account has insufficient cryptocurrency to pay the transaction fee |
| DUPLICATE_TRANSACTION | 11 | This transaction ID is a duplicate of one that was submitted to this node or reached consensus in the last 180 seconds (receipt period) |
| BUSY | 12 | If API is throttled out |
| NOT_SUPPORTED | 13 | The API is not currently supported |
| INVALID_FILE_ID | 14 | The file id is invalid or does not exist |
| INVALID_ACCOUNT_ID | 15 | The account id is invalid or does not exist |
| INVALID_CONTRACT_ID | 16 | The contract id is invalid or does not exist |
| INVALID_TRANSACTION_ID | 17 | Transaction id is not valid |
| RECEIPT_NOT_FOUND | 18 | Receipt for given transaction id does not exist |
| RECORD_NOT_FOUND | 19 | Record for given transaction id does not exist |
| INVALID_SOLIDITY_ID | 20 | The solidity id is invalid or entity with this solidity id does not exist |
| UNKNOWN | 21 | The responding node has submitted the transaction to the network. Its final status is still unknown. |
| SUCCESS | 22 | The transaction succeeded |
| FAIL_INVALID | 23 | There was a system error and the transaction failed because of invalid request parameters. |
| FAIL_FEE | 24 | There was a system error while performing fee calculation, reserved for future. |
| FAIL_BALANCE | 25 | There was a system error while performing balance checks, reserved for future. |
| KEY_REQUIRED | 26 | Key not provided in the transaction body |
| BAD_ENCODING | 27 | Unsupported algorithm/encoding used for keys in the transaction |
| INSUFFICIENT_ACCOUNT_BALANCE | 28 | When the account balance is not sufficient for the transfer |
| INVALID_SOLIDITY_ADDRESS | 29 | During an update transaction when the system is not able to find the Users Solidity address |
| INSUFFICIENT_GAS | 30 | Not enough gas was supplied to execute transaction |
| CONTRACT_SIZE_LIMIT_EXCEEDED | 31 | contract byte code size is over the limit |
| LOCAL_CALL_MODIFICATION_EXCEPTION | 32 | local execution (query) is requested for a function which changes state |
| CONTRACT_REVERT_EXECUTED | 33 | Contract REVERT OPCODE executed |
| CONTRACT_EXECUTION_EXCEPTION | 34 | For any contract execution related error not handled by specific error codes listed above. |
| INVALID_RECEIVING_NODE_ACCOUNT | 35 | In Query validation, account with &#43;ve(amount) value should be Receiving node account, the receiver account should be only one account in the list |
| MISSING_QUERY_HEADER | 36 | Header is missing in Query request |
| ACCOUNT_UPDATE_FAILED | 37 | The update of the account failed |
| INVALID_KEY_ENCODING | 38 | Provided key encoding was not supported by the system |
| NULL_SOLIDITY_ADDRESS | 39 | null solidity address |
| CONTRACT_UPDATE_FAILED | 40 | update of the contract failed |
| INVALID_QUERY_HEADER | 41 | the query header is invalid |
| INVALID_FEE_SUBMITTED | 42 | Invalid fee submitted |
| INVALID_PAYER_SIGNATURE | 43 | Payer signature is invalid |
| KEY_NOT_PROVIDED | 44 | The keys were not provided in the request. |
| INVALID_EXPIRATION_TIME | 45 | Expiration time provided in the transaction was invalid. |
| NO_WACL_KEY | 46 | WriteAccess Control Keys are not provided for the file |
| FILE_CONTENT_EMPTY | 47 | The contents of file are provided as empty. |
| INVALID_ACCOUNT_AMOUNTS | 48 | The crypto transfer credit and debit do not sum equal to 0 |
| EMPTY_TRANSACTION_BODY | 49 | Transaction body provided is empty |
| INVALID_TRANSACTION_BODY | 50 | Invalid transaction body provided |
| INVALID_SIGNATURE_TYPE_MISMATCHING_KEY | 51 | the type of key (base ed25519 key, KeyList, or ThresholdKey) does not match the type of signature (base ed25519 signature, SignatureList, or ThresholdKeySignature) |
| INVALID_SIGNATURE_COUNT_MISMATCHING_KEY | 52 | the number of key (KeyList, or ThresholdKey) does not match that of signature (SignatureList, or ThresholdKeySignature). e.g. if a keyList has 3 base keys, then the corresponding signatureList should also have 3 base signatures. |
| EMPTY_LIVE_HASH_BODY | 53 | the livehash body is empty |
| EMPTY_LIVE_HASH | 54 | the livehash data is missing |
| EMPTY_LIVE_HASH_KEYS | 55 | the keys for a livehash are missing |
| INVALID_LIVE_HASH_SIZE | 56 | the livehash data is not the output of a SHA-384 digest |
| EMPTY_QUERY_BODY | 57 | the query body is empty |
| EMPTY_LIVE_HASH_QUERY | 58 | the crypto livehash query is empty |
| LIVE_HASH_NOT_FOUND | 59 | the livehash is not present |
| ACCOUNT_ID_DOES_NOT_EXIST | 60 | the account id passed has not yet been created. |
| LIVE_HASH_ALREADY_EXISTS | 61 | the livehash already exists for a given account |
| INVALID_FILE_WACL | 62 | File WACL keys are invalid |
| SERIALIZATION_FAILED | 63 | Serialization failure |
| TRANSACTION_OVERSIZE | 64 | The size of the Transaction is greater than transactionMaxBytes |
| TRANSACTION_TOO_MANY_LAYERS | 65 | The Transaction has more than 50 levels |
| CONTRACT_DELETED | 66 | Contract is marked as deleted |
| PLATFORM_NOT_ACTIVE | 67 | the platform node is either disconnected or lagging behind. |
| KEY_PREFIX_MISMATCH | 68 | one public key matches more than one prefixes on the signature map |
| PLATFORM_TRANSACTION_NOT_CREATED | 69 | transaction not created by platform due to large backlog |
| INVALID_RENEWAL_PERIOD | 70 | auto renewal period is not a positive number of seconds |
| INVALID_PAYER_ACCOUNT_ID | 71 | the response code when a smart contract id is passed for a crypto API request |
| ACCOUNT_DELETED | 72 | the account has been marked as deleted |
| FILE_DELETED | 73 | the file has been marked as deleted |
| ACCOUNT_REPEATED_IN_ACCOUNT_AMOUNTS | 74 | same accounts repeated in the transfer account list |
| SETTING_NEGATIVE_ACCOUNT_BALANCE | 75 | attempting to set negative balance value for crypto account |
| OBTAINER_REQUIRED | 76 | when deleting smart contract that has crypto balance either transfer account or transfer smart contract is required |
| OBTAINER_SAME_CONTRACT_ID | 77 | when deleting smart contract that has crypto balance you can not use the same contract id as transferContractId as the one being deleted |
| OBTAINER_DOES_NOT_EXIST | 78 | transferAccountId or transferContractId specified for contract delete does not exist |
| MODIFYING_IMMUTABLE_CONTRACT | 79 | attempting to modify (update or delete a immutable smart contract, i.e. one created without a admin key) |
| FILE_SYSTEM_EXCEPTION | 80 | Unexpected exception thrown by file system functions |
| AUTORENEW_DURATION_NOT_IN_RANGE | 81 | the duration is not a subset of [MINIMUM_AUTORENEW_DURATION,MAXIMUM_AUTORENEW_DURATION] |
| ERROR_DECODING_BYTESTRING | 82 | Decoding the smart contract binary to a byte array failed. Check that the input is a valid hex string. |
| CONTRACT_FILE_EMPTY | 83 | File to create a smart contract was of length zero |
| CONTRACT_BYTECODE_EMPTY | 84 | Bytecode for smart contract is of length zero |
| INVALID_INITIAL_BALANCE | 85 | Attempt to set negative initial balance |
| INVALID_RECEIVE_RECORD_THRESHOLD | 86 | Attempt to set negative receive record threshold |
| INVALID_SEND_RECORD_THRESHOLD | 87 | Attempt to set negative send record threshold |
| ACCOUNT_IS_NOT_GENESIS_ACCOUNT | 88 | Special Account Operations should be performed by only Genesis account, return this code if it is not Genesis Account |
| PAYER_ACCOUNT_UNAUTHORIZED | 89 | The fee payer account doesn&#39;t have permission to submit such Transaction |
| INVALID_FREEZE_TRANSACTION_BODY | 90 | FreezeTransactionBody is invalid |
| FREEZE_TRANSACTION_BODY_NOT_FOUND | 91 | FreezeTransactionBody does not exist |
| TRANSFER_LIST_SIZE_LIMIT_EXCEEDED | 92 | Exceeded the number of accounts (both from and to) allowed for crypto transfer list |
| RESULT_SIZE_LIMIT_EXCEEDED | 93 | Smart contract result size greater than specified maxResultSize |
| NOT_SPECIAL_ACCOUNT | 94 | The payer account is not a special account(account 0.0.55) |
| CONTRACT_NEGATIVE_GAS | 95 | Negative gas was offered in smart contract call |
| CONTRACT_NEGATIVE_VALUE | 96 | Negative value / initial balance was specified in a smart contract call / create |
| INVALID_FEE_FILE | 97 | Failed to update fee file |
| INVALID_EXCHANGE_RATE_FILE | 98 | Failed to update exchange rate file |
| INSUFFICIENT_LOCAL_CALL_GAS | 99 | Payment tendered for contract local call cannot cover both the fee and the gas |
| ENTITY_NOT_ALLOWED_TO_DELETE | 100 | Entities with Entity ID below 1000 are not allowed to be deleted |
| AUTHORIZATION_FAILED | 101 | Violating one of these rules: 1) treasury account can update all entities below 0.0.1000, 2) account 0.0.50 can update all entities from 0.0.51 - 0.0.80, 3) Network Function Master Account A/c 0.0.50 - Update all Network Function accounts &amp; perform all the Network Functions listed below, 4) Network Function Accounts: i) A/c 0.0.55 - Update Address Book files (0.0.101/102), ii) A/c 0.0.56 - Update Fee schedule (0.0.111), iii) A/c 0.0.57 - Update Exchange Rate (0.0.112). |
| FILE_UPLOADED_PROTO_INVALID | 102 | Fee Schedule Proto uploaded but not valid (append or update is required) |
| FILE_UPLOADED_PROTO_NOT_SAVED_TO_DISK | 103 | Fee Schedule Proto uploaded but not valid (append or update is required) |
| FEE_SCHEDULE_FILE_PART_UPLOADED | 104 | Fee Schedule Proto File Part uploaded |
| EXCHANGE_RATE_CHANGE_LIMIT_EXCEEDED | 105 | The change on Exchange Rate exceeds Exchange_Rate_Allowed_Percentage |
| MAX_CONTRACT_STORAGE_EXCEEDED | 106 | Contract permanent storage exceeded the currently allowable limit |
| TRANSFER_ACCOUNT_SAME_AS_DELETE_ACCOUNT | 107 | Transfer Account should not be same as Account to be deleted |
| TOTAL_LEDGER_BALANCE_INVALID | 108 |  |
| EXPIRATION_REDUCTION_NOT_ALLOWED | 110 | The expiration date/time on a smart contract may not be reduced |
| MAX_GAS_LIMIT_EXCEEDED | 111 | Gas exceeded currently allowable gas limit per transaction |
| MAX_FILE_SIZE_EXCEEDED | 112 | File size exceeded the currently allowable limit |
| RECEIVER_SIG_REQUIRED | 113 | When a valid signature is not provided for operations on account with receiverSigRequired=true |
| INVALID_TOPIC_ID | 150 | The Topic ID specified is not in the system. |
| INVALID_ADMIN_KEY | 155 | A provided admin key was invalid. Verify the bytes for an Ed25519 public key are exactly 32 bytes; and the bytes for a compressed ECDSA(secp256k1) key are exactly 33 bytes, with the first byte either 0x02 or 0x03.. |
| INVALID_SUBMIT_KEY | 156 | A provided submit key was invalid. |
| UNAUTHORIZED | 157 | An attempted operation was not authorized (ie - a deleteTopic for a topic with no adminKey). |
| INVALID_TOPIC_MESSAGE | 158 | A ConsensusService message is empty. |
| INVALID_AUTORENEW_ACCOUNT | 159 | The autoRenewAccount specified is not a valid, active account. |
| AUTORENEW_ACCOUNT_NOT_ALLOWED | 160 | An adminKey was not specified on the topic, so there must not be an autoRenewAccount. |
| TOPIC_EXPIRED | 162 | The topic has expired, was not automatically renewed, and is in a 7 day grace period before the topic will be deleted unrecoverably. This error response code will not be returned until autoRenew functionality is supported by HAPI. |
| INVALID_CHUNK_NUMBER | 163 | chunk number must be from 1 to total (chunks) inclusive. |
| INVALID_CHUNK_TRANSACTION_ID | 164 | For every chunk, the payer account that is part of initialTransactionID must match the Payer Account of this transaction. The entire initialTransactionID should match the transactionID of the first chunk, but this is not checked or enforced by Hedera except when the chunk number is 1. |
| ACCOUNT_FROZEN_FOR_TOKEN | 165 | Account is frozen and cannot transact with the token |
| TOKENS_PER_ACCOUNT_LIMIT_EXCEEDED | 166 | An involved account already has more than &lt;tt&gt;tokens.maxPerAccount&lt;/tt&gt; associations with non-deleted tokens. |
| INVALID_TOKEN_ID | 167 | The token is invalid or does not exist |
| INVALID_TOKEN_DECIMALS | 168 | Invalid token decimals |
| INVALID_TOKEN_INITIAL_SUPPLY | 169 | Invalid token initial supply |
| INVALID_TREASURY_ACCOUNT_FOR_TOKEN | 170 | Treasury Account does not exist or is deleted |
| INVALID_TOKEN_SYMBOL | 171 | Token Symbol is not UTF-8 capitalized alphabetical string |
| TOKEN_HAS_NO_FREEZE_KEY | 172 | Freeze key is not set on token |
| TRANSFERS_NOT_ZERO_SUM_FOR_TOKEN | 173 | Amounts in transfer list are not net zero |
| MISSING_TOKEN_SYMBOL | 174 | A token symbol was not provided |
| TOKEN_SYMBOL_TOO_LONG | 175 | The provided token symbol was too long |
| ACCOUNT_KYC_NOT_GRANTED_FOR_TOKEN | 176 | KYC must be granted and account does not have KYC granted |
| TOKEN_HAS_NO_KYC_KEY | 177 | KYC key is not set on token |
| INSUFFICIENT_TOKEN_BALANCE | 178 | Token balance is not sufficient for the transaction |
| TOKEN_WAS_DELETED | 179 | Token transactions cannot be executed on deleted token |
| TOKEN_HAS_NO_SUPPLY_KEY | 180 | Supply key is not set on token |
| TOKEN_HAS_NO_WIPE_KEY | 181 | Wipe key is not set on token |
| INVALID_TOKEN_MINT_AMOUNT | 182 | The requested token mint amount would cause an invalid total supply |
| INVALID_TOKEN_BURN_AMOUNT | 183 | The requested token burn amount would cause an invalid total supply |
| TOKEN_NOT_ASSOCIATED_TO_ACCOUNT | 184 | A required token-account relationship is missing |
| CANNOT_WIPE_TOKEN_TREASURY_ACCOUNT | 185 | The target of a wipe operation was the token treasury account |
| INVALID_KYC_KEY | 186 | The provided KYC key was invalid. |
| INVALID_WIPE_KEY | 187 | The provided wipe key was invalid. |
| INVALID_FREEZE_KEY | 188 | The provided freeze key was invalid. |
| INVALID_SUPPLY_KEY | 189 | The provided supply key was invalid. |
| MISSING_TOKEN_NAME | 190 | Token Name is not provided |
| TOKEN_NAME_TOO_LONG | 191 | Token Name is too long |
| INVALID_WIPING_AMOUNT | 192 | The provided wipe amount must not be negative, zero or bigger than the token holder balance |
| TOKEN_IS_IMMUTABLE | 193 | Token does not have Admin key set, thus update/delete transactions cannot be performed |
| TOKEN_ALREADY_ASSOCIATED_TO_ACCOUNT | 194 | An &lt;tt&gt;associateToken&lt;/tt&gt; operation specified a token already associated to the account |
| TRANSACTION_REQUIRES_ZERO_TOKEN_BALANCES | 195 | An attempted operation is invalid until all token balances for the target account are zero |
| ACCOUNT_IS_TREASURY | 196 | An attempted operation is invalid because the account is a treasury |
| TOKEN_ID_REPEATED_IN_TOKEN_LIST | 197 | Same TokenIDs present in the token list |
| TOKEN_TRANSFER_LIST_SIZE_LIMIT_EXCEEDED | 198 | Exceeded the number of token transfers (both from and to) allowed for token transfer list |
| EMPTY_TOKEN_TRANSFER_BODY | 199 | TokenTransfersTransactionBody has no TokenTransferList |
| EMPTY_TOKEN_TRANSFER_ACCOUNT_AMOUNTS | 200 | TokenTransfersTransactionBody has a TokenTransferList with no AccountAmounts |
| INVALID_SCHEDULE_ID | 201 | The Scheduled entity does not exist; or has now expired, been deleted, or been executed |
| SCHEDULE_IS_IMMUTABLE | 202 | The Scheduled entity cannot be modified. Admin key not set |
| INVALID_SCHEDULE_PAYER_ID | 203 | The provided Scheduled Payer does not exist |
| INVALID_SCHEDULE_ACCOUNT_ID | 204 | The Schedule Create Transaction TransactionID account does not exist |
| NO_NEW_VALID_SIGNATURES | 205 | The provided sig map did not contain any new valid signatures from required signers of the scheduled transaction |
| UNRESOLVABLE_REQUIRED_SIGNERS | 206 | The required signers for a scheduled transaction cannot be resolved, for example because they do not exist or have been deleted |
| SCHEDULED_TRANSACTION_NOT_IN_WHITELIST | 207 | Only whitelisted transaction types may be scheduled |
| SOME_SIGNATURES_WERE_INVALID | 208 | At least one of the signatures in the provided sig map did not represent a valid signature for any required signer |
| TRANSACTION_ID_FIELD_NOT_ALLOWED | 209 | The scheduled field in the TransactionID may not be set to true |
| IDENTICAL_SCHEDULE_ALREADY_CREATED | 210 | A schedule already exists with the same identifying fields of an attempted ScheduleCreate (that is, all fields other than scheduledPayerAccountID) |
| INVALID_ZERO_BYTE_IN_STRING | 211 | A string field in the transaction has a UTF-8 encoding with the prohibited zero byte |
| SCHEDULE_ALREADY_DELETED | 212 | A schedule being signed or deleted has already been deleted |
| SCHEDULE_ALREADY_EXECUTED | 213 | A schedule being signed or deleted has already been executed |
| MESSAGE_SIZE_TOO_LARGE | 214 | ConsensusSubmitMessage request&#39;s message size is larger than allowed. |
| OPERATION_REPEATED_IN_BUCKET_GROUPS | 215 | An operation was assigned to more than one throttle group in a given bucket |
| BUCKET_CAPACITY_OVERFLOW | 216 | The capacity needed to satisfy all opsPerSec groups in a bucket overflowed a signed 8-byte integral type |
| NODE_CAPACITY_NOT_SUFFICIENT_FOR_OPERATION | 217 | Given the network size in the address book, the node-level capacity for an operation would never be enough to accept a single request; usually means a bucket burstPeriod should be increased |
| BUCKET_HAS_NO_THROTTLE_GROUPS | 218 | A bucket was defined without any throttle groups |
| THROTTLE_GROUP_HAS_ZERO_OPS_PER_SEC | 219 | A throttle group was granted zero opsPerSec |
| SUCCESS_BUT_MISSING_EXPECTED_OPERATION | 220 | The throttle definitions file was updated, but some supported operations were not assigned a bucket |
| UNPARSEABLE_THROTTLE_DEFINITIONS | 221 | The new contents for the throttle definitions system file were not valid protobuf |
| INVALID_THROTTLE_DEFINITIONS | 222 | The new throttle definitions system file were invalid, and no more specific error could be divined |
| ACCOUNT_EXPIRED_AND_PENDING_REMOVAL | 223 | The transaction references an account which has passed its expiration without renewal funds available, and currently remains in the ledger only because of the grace period given to expired entities |
| INVALID_TOKEN_MAX_SUPPLY | 224 | Invalid token max supply |
| INVALID_TOKEN_NFT_SERIAL_NUMBER | 225 | Invalid token nft serial number |
| INVALID_NFT_ID | 226 | Invalid nft id |
| METADATA_TOO_LONG | 227 | Nft metadata is too long |
| BATCH_SIZE_LIMIT_EXCEEDED | 228 | Repeated operations count exceeds the limit |
| INVALID_QUERY_RANGE | 229 | The range of data to be gathered is out of the set boundaries |
| FRACTION_DIVIDES_BY_ZERO | 230 | A custom fractional fee set a denominator of zero |
| INSUFFICIENT_PAYER_BALANCE_FOR_CUSTOM_FEE | 231 | The transaction payer could not afford a custom fee |
| CUSTOM_FEES_LIST_TOO_LONG | 232 | More than 10 custom fees were specified |
| INVALID_CUSTOM_FEE_COLLECTOR | 233 | Any of the feeCollector accounts for customFees is invalid |
| INVALID_TOKEN_ID_IN_CUSTOM_FEES | 234 | Any of the token Ids in customFees is invalid |
| TOKEN_NOT_ASSOCIATED_TO_FEE_COLLECTOR | 235 | Any of the token Ids in customFees are not associated to feeCollector |
| TOKEN_MAX_SUPPLY_REACHED | 236 | A token cannot have more units minted due to its configured supply ceiling |
| SENDER_DOES_NOT_OWN_NFT_SERIAL_NO | 237 | The transaction attempted to move an NFT serial number from an account other than its owner |
| CUSTOM_FEE_NOT_FULLY_SPECIFIED | 238 | A custom fee schedule entry did not specify either a fixed or fractional fee |
| CUSTOM_FEE_MUST_BE_POSITIVE | 239 | Only positive fees may be assessed at this time |
| TOKEN_HAS_NO_FEE_SCHEDULE_KEY | 240 | Fee schedule key is not set on token |
| CUSTOM_FEE_OUTSIDE_NUMERIC_RANGE | 241 | A fractional custom fee exceeded the range of a 64-bit signed integer |
| ROYALTY_FRACTION_CANNOT_EXCEED_ONE | 242 | A royalty cannot exceed the total fungible value exchanged for an NFT |
| FRACTIONAL_FEE_MAX_AMOUNT_LESS_THAN_MIN_AMOUNT | 243 | Each fractional custom fee must have its maximum_amount, if specified, at least its minimum_amount |
| CUSTOM_SCHEDULE_ALREADY_HAS_NO_FEES | 244 | A fee schedule update tried to clear the custom fees from a token whose fee schedule was already empty |
| CUSTOM_FEE_DENOMINATION_MUST_BE_FUNGIBLE_COMMON | 245 | Only tokens of type FUNGIBLE_COMMON can be used to as fee schedule denominations |
| CUSTOM_FRACTIONAL_FEE_ONLY_ALLOWED_FOR_FUNGIBLE_COMMON | 246 | Only tokens of type FUNGIBLE_COMMON can have fractional fees |
| INVALID_CUSTOM_FEE_SCHEDULE_KEY | 247 | The provided custom fee schedule key was invalid |
| INVALID_TOKEN_MINT_METADATA | 248 | The requested token mint metadata was invalid |
| INVALID_TOKEN_BURN_METADATA | 249 | The requested token burn metadata was invalid |
| CURRENT_TREASURY_STILL_OWNS_NFTS | 250 | The treasury for a unique token cannot be changed until it owns no NFTs |
| ACCOUNT_STILL_OWNS_NFTS | 251 | An account cannot be dissociated from a unique token if it owns NFTs for the token |
| TREASURY_MUST_OWN_BURNED_NFT | 252 | A NFT can only be burned when owned by the unique token&#39;s treasury |
| ACCOUNT_DOES_NOT_OWN_WIPED_NFT | 253 | An account did not own the NFT to be wiped |
| ACCOUNT_AMOUNT_TRANSFERS_ONLY_ALLOWED_FOR_FUNGIBLE_COMMON | 254 | An AccountAmount token transfers list referenced a token type other than FUNGIBLE_COMMON |
| MAX_NFTS_IN_PRICE_REGIME_HAVE_BEEN_MINTED | 255 | All the NFTs allowed in the current price regime have already been minted |
| PAYER_ACCOUNT_DELETED | 256 | The payer account has been marked as deleted |
| CUSTOM_FEE_CHARGING_EXCEEDED_MAX_RECURSION_DEPTH | 257 | The reference chain of custom fees for a transferred token exceeded the maximum length of 2 |
| CUSTOM_FEE_CHARGING_EXCEEDED_MAX_ACCOUNT_AMOUNTS | 258 | More than 20 balance adjustments were to satisfy a CryptoTransfer and its implied custom fee payments |
| INSUFFICIENT_SENDER_ACCOUNT_BALANCE_FOR_CUSTOM_FEE | 259 | The sender account in the token transfer transaction could not afford a custom fee |
| SERIAL_NUMBER_LIMIT_REACHED | 260 | Currently no more than 4,294,967,295 NFTs may be minted for a given unique token type |
| CUSTOM_ROYALTY_FEE_ONLY_ALLOWED_FOR_NON_FUNGIBLE_UNIQUE | 261 | Only tokens of type NON_FUNGIBLE_UNIQUE can have royalty fees |
| NO_REMAINING_AUTOMATIC_ASSOCIATIONS | 262 | The account has reached the limit on the automatic associations count. |
| EXISTING_AUTOMATIC_ASSOCIATIONS_EXCEED_GIVEN_LIMIT | 263 | Already existing automatic associations are more than the new maximum automatic associations. |
| REQUESTED_NUM_AUTOMATIC_ASSOCIATIONS_EXCEEDS_ASSOCIATION_LIMIT | 264 | Cannot set the number of automatic associations for an account more than the maximum allowed token associations &lt;tt&gt;tokens.maxPerAccount&lt;/tt&gt;. |
| TOKEN_IS_PAUSED | 265 | Token is paused. This Token cannot be a part of any kind of Transaction until unpaused. |
| TOKEN_HAS_NO_PAUSE_KEY | 266 | Pause key is not set on token |
| INVALID_PAUSE_KEY | 267 | The provided pause key was invalid |
| FREEZE_UPDATE_FILE_DOES_NOT_EXIST | 268 | The update file in a freeze transaction body must exist. |
| FREEZE_UPDATE_FILE_HASH_DOES_NOT_MATCH | 269 | The hash of the update file in a freeze transaction body must match the in-memory hash. |
| NO_UPGRADE_HAS_BEEN_PREPARED | 270 | A FREEZE_UPGRADE transaction was handled with no previous update prepared. |
| NO_FREEZE_IS_SCHEDULED | 271 | A FREEZE_ABORT transaction was handled with no scheduled freeze. |
| UPDATE_FILE_HASH_CHANGED_SINCE_PREPARE_UPGRADE | 272 | The update file hash when handling a FREEZE_UPGRADE transaction differs from the file hash at the time of handling the PREPARE_UPGRADE transaction. |
| FREEZE_START_TIME_MUST_BE_FUTURE | 273 | The given freeze start time was in the (consensus) past. |
| PREPARED_UPDATE_FILE_IS_IMMUTABLE | 274 | The prepared update file cannot be updated or appended until either the upgrade has been completed, or a FREEZE_ABORT has been handled. |
| FREEZE_ALREADY_SCHEDULED | 275 | Once a freeze is scheduled, it must be aborted before any other type of freeze can can be performed. |
| FREEZE_UPGRADE_IN_PROGRESS | 276 | If an NMT upgrade has been prepared, the following operation must be a FREEZE_UPGRADE. (To issue a FREEZE_ONLY, submit a FREEZE_ABORT first.) |
| UPDATE_FILE_ID_DOES_NOT_MATCH_PREPARED | 277 | If an NMT upgrade has been prepared, the subsequent FREEZE_UPGRADE transaction must confirm the id of the file to be used in the upgrade. |
| UPDATE_FILE_HASH_DOES_NOT_MATCH_PREPARED | 278 | If an NMT upgrade has been prepared, the subsequent FREEZE_UPGRADE transaction must confirm the hash of the file to be used in the upgrade. |
| CONSENSUS_GAS_EXHAUSTED | 279 | Consensus throttle did not allow execution of this transaction. System is throttled at consensus level. |
| REVERTED_SUCCESS | 280 | A precompiled contract succeeded, but was later reverted. |
| MAX_STORAGE_IN_PRICE_REGIME_HAS_BEEN_USED | 281 | All contract storage allocated to the current price regime has been consumed. |
| INVALID_ALIAS_KEY | 282 | An alias used in a CryptoTransfer transaction is not the serialization of a primitive Key message--that is, a Key with a single Ed25519 or ECDSA(secp256k1) public key and no unknown protobuf fields. |
| UNEXPECTED_TOKEN_DECIMALS | 283 | A fungible token transfer expected a different number of decimals than the involved type actually has. |
| INVALID_PROXY_ACCOUNT_ID | 284 | The proxy account id is invalid or does not exist. |
| INVALID_TRANSFER_ACCOUNT_ID | 285 | The transfer account id in CryptoDelete transaction is invalid or does not exist. |
| INVALID_FEE_COLLECTOR_ACCOUNT_ID | 286 | The fee collector account id in TokenFeeScheduleUpdate is invalid or does not exist. |
| ALIAS_IS_IMMUTABLE | 287 | The alias already set on an account cannot be updated using CryptoUpdate transaction. |
| SPENDER_ACCOUNT_SAME_AS_OWNER | 288 | An approved allowance specifies a spender account that is the same as the hbar/token owner account. |
| AMOUNT_EXCEEDS_TOKEN_MAX_SUPPLY | 289 | The establishment or adjustment of an approved allowance cause the token allowance to exceed the token maximum supply. |
| NEGATIVE_ALLOWANCE_AMOUNT | 290 | The specified amount for an approved allowance cannot be negative. |
| CANNOT_APPROVE_FOR_ALL_FUNGIBLE_COMMON | 291 | The approveForAll flag cannot be set for a fungible token. |
| SPENDER_DOES_NOT_HAVE_ALLOWANCE | 292 | The spender does not have an existing approved allowance with the hbar/token owner. |
| AMOUNT_EXCEEDS_ALLOWANCE | 293 | The transfer amount exceeds the current approved allowance for the spender account. |
| MAX_ALLOWANCES_EXCEEDED | 294 | The payer account of an approveAllowances or adjustAllowance transaction is attempting to go beyond the maximum allowed number of allowances. |
| EMPTY_ALLOWANCES | 295 | No allowances have been specified in the approval transaction. |
| SPENDER_ACCOUNT_REPEATED_IN_ALLOWANCES | 296 | Spender is repeated more than once in Crypto or Token or NFT allowance lists in a single CryptoApproveAllowance transaction. |
| REPEATED_SERIAL_NUMS_IN_NFT_ALLOWANCES | 297 | Serial numbers are repeated in nft allowance for a single spender account |
| FUNGIBLE_TOKEN_IN_NFT_ALLOWANCES | 298 | Fungible common token used in NFT allowances |
| NFT_IN_FUNGIBLE_TOKEN_ALLOWANCES | 299 | Non fungible token used in fungible token allowances |
| INVALID_ALLOWANCE_OWNER_ID | 300 | The account id specified as the owner is invalid or does not exist. |
| INVALID_ALLOWANCE_SPENDER_ID | 301 | The account id specified as the spender is invalid or does not exist. |
| REPEATED_ALLOWANCES_TO_DELETE | 302 | [Deprecated] If the CryptoDeleteAllowance transaction has repeated crypto or token or Nft allowances to delete. |
| INVALID_DELEGATING_SPENDER | 303 | If the account Id specified as the delegating spender is invalid or does not exist. |
| DELEGATING_SPENDER_CANNOT_GRANT_APPROVE_FOR_ALL | 304 | The delegating Spender cannot grant approveForAll allowance on a NFT token type for another spender. |
| DELEGATING_SPENDER_DOES_NOT_HAVE_APPROVE_FOR_ALL | 305 | The delegating Spender cannot grant allowance on a NFT serial for another spender as it doesnt not have approveForAll granted on token-owner. |
| SCHEDULE_EXPIRATION_TIME_TOO_FAR_IN_FUTURE | 306 | The scheduled transaction could not be created because it&#39;s expiration_time was too far in the future. |
| SCHEDULE_EXPIRATION_TIME_MUST_BE_HIGHER_THAN_CONSENSUS_TIME | 307 | The scheduled transaction could not be created because it&#39;s expiration_time was less than or equal to the consensus time. |
| SCHEDULE_FUTURE_THROTTLE_EXCEEDED | 308 | The scheduled transaction could not be created because it would cause throttles to be violated on the specified expiration_time. |
| SCHEDULE_FUTURE_GAS_LIMIT_EXCEEDED | 309 | The scheduled transaction could not be created because it would cause the gas limit to be violated on the specified expiration_time. |
| INVALID_ETHEREUM_TRANSACTION | 310 | The ethereum transaction either failed parsing or failed signature validation, or some other EthereumTransaction error not covered by another response code. |
| WRONG_CHAIN_ID | 311 | EthereumTransaction was signed against a chainId that this network does not support. |
| WRONG_NONCE | 312 | This transaction specified an ethereumNonce that is not the current ethereumNonce of the account. |
| ACCESS_LIST_UNSUPPORTED | 313 | The ethereum transaction specified an access list, which the network does not support. |
| SCHEDULE_PENDING_EXPIRATION | 314 | A schedule being signed or deleted has passed it&#39;s expiration date and is pending execution if needed and then expiration. |
| CONTRACT_IS_TOKEN_TREASURY | 315 | A selfdestruct or ContractDelete targeted a contract that is a token treasury. |
| CONTRACT_HAS_NON_ZERO_TOKEN_BALANCES | 316 | A selfdestruct or ContractDelete targeted a contract with non-zero token balances. |
| CONTRACT_EXPIRED_AND_PENDING_REMOVAL | 317 | A contract referenced by a transaction is &#34;detached&#34;; that is, expired and lacking any hbar funds for auto-renewal payment---but still within its post-expiry grace period. |
| CONTRACT_HAS_NO_AUTO_RENEW_ACCOUNT | 318 | A ContractUpdate requested removal of a contract&#39;s auto-renew account, but that contract has no auto-renew account. |
| PERMANENT_REMOVAL_REQUIRES_SYSTEM_INITIATION | 319 | A delete transaction submitted via HAPI set permanent_removal=true |
| PROXY_ACCOUNT_ID_FIELD_IS_DEPRECATED | 320 | A CryptoCreate or ContractCreate used the deprecated proxyAccountID field. |
| SELF_STAKING_IS_NOT_ALLOWED | 321 | An account set the staked_account_id to itself in CryptoUpdate or ContractUpdate transactions. |
| INVALID_STAKING_ID | 322 | The staking account id or staking node id given is invalid or does not exist. |
| STAKING_NOT_ENABLED | 323 | Native staking, while implemented, has not yet enabled by the council. |
| INVALID_PRNG_RANGE | 324 | The range provided in UtilPrng transaction is negative. |
| MAX_ENTITIES_IN_PRICE_REGIME_HAVE_BEEN_CREATED | 325 | The maximum number of entities allowed in the current price regime have been created. |
| INVALID_FULL_PREFIX_SIGNATURE_FOR_PRECOMPILE | 326 | The full prefix signature for precompile is not valid |
| INSUFFICIENT_BALANCES_FOR_STORAGE_RENT | 327 | The combined balances of a contract and its auto-renew account (if any) did not cover the rent charged for net new storage used in a transaction. |
| MAX_CHILD_RECORDS_EXCEEDED | 328 | A contract transaction tried to use more than the allowed number of child records, via either system contract records or internal contract creations. |
| INSUFFICIENT_BALANCES_FOR_RENEWAL_FEES | 329 | The combined balances of a contract and its auto-renew account (if any) or balance of an account did not cover the auto-renewal fees in a transaction. |
| TRANSACTION_HAS_UNKNOWN_FIELDS | 330 | A transaction&#39;s protobuf message includes unknown fields; could mean that a client expects not-yet-released functionality to be available. |
| ACCOUNT_IS_IMMUTABLE | 331 | The account cannot be modified. Account&#39;s key is not set |
| ALIAS_ALREADY_ASSIGNED | 332 | An alias that is assigned to an account or contract cannot be assigned to another account or contract. |
| INVALID_METADATA_KEY | 333 | A provided metadata key was invalid. Verification includes, for example, checking the size of Ed25519 and ECDSA(secp256k1) public keys. |
| TOKEN_HAS_NO_METADATA_KEY | 334 | Metadata key is not set on token |
| MISSING_TOKEN_METADATA | 335 | Token Metadata is not provided |
| MISSING_SERIAL_NUMBERS | 336 | NFT serial numbers are missing in the TokenUpdateNftsTransactionBody |
| TOKEN_HAS_NO_ADMIN_KEY | 337 | Admin key is not set on token |
| NODE_DELETED | 338 | A transaction failed because the consensus node identified is deleted from the address book. |
| INVALID_NODE_ID | 339 | A transaction failed because the consensus node identified is not valid or does not exist in state. |
| INVALID_GOSSIP_ENDPOINT | 340 | A transaction failed because one or more entries in the list of service endpoints for the `gossip_endpoint` field is invalid.&lt;br/&gt; The most common cause for this response is a service endpoint that has the domain name (DNS) set rather than address and port. |
| INVALID_NODE_ACCOUNT_ID | 341 | A transaction failed because the node account identifier provided does not exist or is not valid.&lt;br/&gt; One common source of this error is providing a node account identifier using the &#34;alias&#34; form rather than &#34;numeric&#34; form. It is also used for atomic batch transaction for child transaction if the node account id is not 0.0.0. |
| INVALID_NODE_DESCRIPTION | 342 | A transaction failed because the description field cannot be encoded as UTF-8 or is more than 100 bytes when encoded. |
| INVALID_SERVICE_ENDPOINT | 343 | A transaction failed because one or more entries in the list of service endpoints for the `service_endpoint` field is invalid.&lt;br/&gt; The most common cause for this response is a service endpoint that has the domain name (DNS) set rather than address and port. |
| INVALID_GOSSIP_CA_CERTIFICATE | 344 | A transaction failed because the TLS certificate provided for the node is missing or invalid. &lt;p&gt; #### Probable Causes The certificate MUST be a TLS certificate of a type permitted for gossip signatures.&lt;br/&gt; The value presented MUST be a UTF-8 NFKD encoding of the TLS certificate.&lt;br/&gt; The certificate encoded MUST be in PEM format.&lt;br/&gt; The `gossip_ca_certificate` field is REQUIRED and MUST NOT be empty. |
| INVALID_GRPC_CERTIFICATE | 345 | A transaction failed because the hash provided for the gRPC certificate is present but invalid. &lt;p&gt; #### Probable Causes The `grpc_certificate_hash` MUST be a SHA-384 hash.&lt;br/&gt; The input hashed MUST be a UTF-8 NFKD encoding of the actual TLS certificate.&lt;br/&gt; The certificate to be encoded MUST be in PEM format. |
| INVALID_MAX_AUTO_ASSOCIATIONS | 346 | The maximum automatic associations value is not valid.&lt;br/&gt; The most common cause for this error is a value less than `-1`. |
| MAX_NODES_CREATED | 347 | The maximum number of nodes allowed in the address book have been created. |
| IP_FQDN_CANNOT_BE_SET_FOR_SAME_ENDPOINT | 348 | In ServiceEndpoint, domain_name and ipAddressV4 are mutually exclusive |
| GOSSIP_ENDPOINT_CANNOT_HAVE_FQDN | 349 | Fully qualified domain name is not allowed in gossip_endpoint |
| FQDN_SIZE_TOO_LARGE | 350 | In ServiceEndpoint, domain_name size too large |
| INVALID_ENDPOINT | 351 | ServiceEndpoint is invalid |
| GOSSIP_ENDPOINTS_EXCEEDED_LIMIT | 352 | The number of gossip endpoints exceeds the limit |
| TOKEN_REFERENCE_REPEATED | 353 | The transaction attempted to use duplicate `TokenReference`.&lt;br/&gt; This affects `TokenReject` attempting to reject same token reference more than once. |
| INVALID_OWNER_ID | 354 | The account id specified as the owner in `TokenReject` is invalid or does not exist. |
| TOKEN_REFERENCE_LIST_SIZE_LIMIT_EXCEEDED | 355 | The transaction attempted to use more than the allowed number of `TokenReference`. |
| SERVICE_ENDPOINTS_EXCEEDED_LIMIT | 356 | The number of service endpoints exceeds the limit |
| INVALID_IPV4_ADDRESS | 357 | The IPv4 address is invalid |
| EMPTY_TOKEN_REFERENCE_LIST | 358 | The transaction attempted to use empty `TokenReference` list. |
| UPDATE_NODE_ACCOUNT_NOT_ALLOWED | 359 | The node account is not allowed to be updated |
| TOKEN_HAS_NO_METADATA_OR_SUPPLY_KEY | 360 | The token has no metadata or supply key |
| EMPTY_PENDING_AIRDROP_ID_LIST | 361 | The list of `PendingAirdropId`s is empty and MUST NOT be empty. |
| PENDING_AIRDROP_ID_REPEATED | 362 | A `PendingAirdropId` is repeated in a `claim` or `cancel` transaction. |
| PENDING_AIRDROP_ID_LIST_TOO_LONG | 363 | The number of `PendingAirdropId` values in the list exceeds the maximum allowable number. |
| PENDING_NFT_AIRDROP_ALREADY_EXISTS | 364 | A pending airdrop already exists for the specified NFT. |
| ACCOUNT_HAS_PENDING_AIRDROPS | 365 | The identified account is sender for one or more pending airdrop(s) and cannot be deleted. &lt;p&gt; The requester SHOULD cancel all pending airdrops before resending this transaction. |
| THROTTLED_AT_CONSENSUS | 366 | Consensus throttle did not allow execution of this transaction.&lt;br/&gt; The transaction should be retried after a modest delay. |
| INVALID_PENDING_AIRDROP_ID | 367 | The provided pending airdrop id is invalid.&lt;br/&gt; This pending airdrop MAY already be claimed or cancelled. &lt;p&gt; The client SHOULD query a mirror node to determine the current status of the pending airdrop. |
| TOKEN_AIRDROP_WITH_FALLBACK_ROYALTY | 368 | The token to be airdropped has a fallback royalty fee and cannot be sent or claimed via an airdrop transaction. |
| INVALID_TOKEN_IN_PENDING_AIRDROP | 369 | This airdrop claim is for a pending airdrop with an invalid token.&lt;br/&gt; The token might be deleted, or the sender may not have enough tokens to fulfill the offer. &lt;p&gt; The client SHOULD query mirror node to determine the status of the pending airdrop and whether the sender can fulfill the offer. |
| SCHEDULE_EXPIRY_IS_BUSY | 370 | A scheduled transaction configured to wait for expiry to execute was given an expiry time at which there is already too many transactions scheduled to expire; its creation must be retried with a different expiry. |
| INVALID_GRPC_CERTIFICATE_HASH | 371 | The provided gRPC certificate hash is invalid. |
| MISSING_EXPIRY_TIME | 372 | A scheduled transaction configured to wait for expiry to execute was not given an explicit expiration time. |
| NO_SCHEDULING_ALLOWED_AFTER_SCHEDULED_RECURSION | 373 | A contract operation attempted to schedule another transaction after it had already scheduled a recursive contract call. |
| RECURSIVE_SCHEDULING_LIMIT_REACHED | 374 | A contract can schedule recursive calls a finite number of times (this is approximately four million times with typical network configuration.) |
| WAITING_FOR_LEDGER_ID | 375 | The target network is waiting for the ledger ID to be set, which is a side effect of finishing the network&#39;s TSS construction. |
| MAX_ENTRIES_FOR_FEE_EXEMPT_KEY_LIST_EXCEEDED | 376 | The provided fee exempt key list size exceeded the limit. |
| FEE_EXEMPT_KEY_LIST_CONTAINS_DUPLICATED_KEYS | 377 | The provided fee exempt key list contains duplicated keys. |
| INVALID_KEY_IN_FEE_EXEMPT_KEY_LIST | 378 | The provided fee exempt key list contains an invalid key. |
| INVALID_FEE_SCHEDULE_KEY | 379 | The provided fee schedule key contains an invalid key. |
| FEE_SCHEDULE_KEY_CANNOT_BE_UPDATED | 380 | If a fee schedule key is not set when we create a topic we cannot add it on update. |
| FEE_SCHEDULE_KEY_NOT_SET | 381 | If the topic&#39;s custom fees are updated the topic SHOULD have a fee schedule key |
| MAX_CUSTOM_FEE_LIMIT_EXCEEDED | 382 | The fee amount is exceeding the amount that the payer is willing to pay. |
| NO_VALID_MAX_CUSTOM_FEE | 383 | There are no corresponding custom fees. |
| INVALID_MAX_CUSTOM_FEES | 384 | The provided list contains invalid max custom fee. |
| DUPLICATE_DENOMINATION_IN_MAX_CUSTOM_FEE_LIST | 385 | The provided max custom fee list contains fees with duplicate denominations. |
| DUPLICATE_ACCOUNT_ID_IN_MAX_CUSTOM_FEE_LIST | 386 | The provided max custom fee list contains fees with duplicate account id. |
| MAX_CUSTOM_FEES_IS_NOT_SUPPORTED | 387 | Max custom fees list is not supported for this operation. |
| BATCH_LIST_EMPTY | 388 | The list of batch transactions is empty |
| BATCH_LIST_CONTAINS_DUPLICATES | 389 | The list of batch transactions contains duplicated transactions |
| BATCH_TRANSACTION_IN_BLACKLIST | 390 | The list of batch transactions contains a transaction type that is in the AtomicBatch blacklist as configured in the network. |
| INNER_TRANSACTION_FAILED | 391 | The inner transaction of a batch transaction failed |
| MISSING_BATCH_KEY | 392 | The inner transaction of a batch transaction is missing a batch key |
| BATCH_KEY_SET_ON_NON_INNER_TRANSACTION | 393 | The batch key is set for a non batch transaction |
| INVALID_BATCH_KEY | 394 | The batch key is not valid |
| SCHEDULE_EXPIRY_NOT_CONFIGURABLE | 395 | The provided schedule expiry time is not configurable. |
| CREATING_SYSTEM_ENTITIES | 396 | The network just started at genesis and is creating system entities. |


 

 

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

