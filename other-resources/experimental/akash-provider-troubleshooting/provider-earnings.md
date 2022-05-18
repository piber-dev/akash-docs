# Provider Earnings

Use the verifications included in this section for the following purposes:

* [AKT Total Earned by Provider](provider-earnings.md#akt-total-earned-by-provider)
* [AKT Total Earning Potential Per Active Deployment](provider-earnings.md#akt-total-earning-potential-per-active-deployment)

## AKT Total Earned by Provider

### Command Template

Issue the commands in this section from any machine that has the [Akash CLI](../../../guides/cli/detailed-steps/) installed.

```
akash query market lease list --provider <PROVIDER-ADDRESS> --page 1 --limit 1000 -o json | jq -r '([.leases[].escrow_payment.withdrawn.amount|tonumber] | add) / pow(10;6)'
```

### **Example Command Use**

```
akash query market lease list --provider akash1yvu4hhnvs84v4sv53mzu5ntf7fxf4cfup9s22j --page 1 --limit 1000 -o json | jq -r '([.leases[].escrow_payment.withdrawn.amount|tonumber] | add) / pow(10;6)'
```

### Example Output

```
akash query market lease list --provider akash1yvu4hhnvs84v4sv53mzu5ntf7fxf4cfup9s22j --page 1 --limit 1000 -o json | jq -r '([.leases[].escrow_payment.withdrawn.amount|tonumber] | add) / pow(10;6)'

8.003348
```

## AKT Total Earning Potential Per Active Deployment

### Command Syntax

Issue the commands in this section from any machine that has the [Akash CLI](../../../guides/cli/detailed-steps/) installed.

```
akash query market lease list --provider <PROVIDER-ADDRESS> --gseq 0 --oseq 0 --page 1 --limit 100 --state active -o json | jq -r '["owner","dseq","gseq","oseq","rate","monthly","USD"], (.leases[] | [(.lease.lease_id | .owner, .dseq, .gseq, .oseq), (.escrow_payment | .rate.amount, (.rate.amount|tonumber), (.rate.amount|tonumber))]) | @csv' | awk -F ',' '{if (NR==1) {$1=$1; printf $0"\n"} else {$6=(($6*((60/6.706)*60*24*30.436875))/10^6); $7=(($7*((60/6.706)*60*24*30.436875))/10^6)*1.79; print $0}}' | column -t
```

### Example Command Use

```
akash query market lease list --provider akash1yvu4hhnvs84v4sv53mzu5ntf7fxf4cfup9s22j --gseq 0 --oseq 0 --page 1 --limit 100 --state active -o json | jq -r '["owner","dseq","gseq","oseq","rate","monthly","USD"], (.leases[] | [(.lease.lease_id | .owner, .dseq, .gseq, .oseq), (.escrow_payment | .rate.amount, (.rate.amount|tonumber), (.rate.amount|tonumber))]) | @csv' | awk -F ',' '{if (NR==1) {$1=$1; printf $0"\n"} else {$6=(($6*((60/6.706)*60*24*30.436875))/10^6); $7=(($7*((60/6.706)*60*24*30.436875))/10^6)*1.79; print $0}}' | column -t
```

### Example Output

```
"owner"                                         "dseq"     "gseq"  "oseq"  "rate"                  "monthly"  "USD"

"akash1n44zc8l6gfm0hpydldndpg8n05xjjwmuahc6nn"  "5850047"  1       1       "4.901120000000000000"  1.92197    3.44032
"akash1n44zc8l6gfm0hpydldndpg8n05xjjwmuahc6nn"  "5850470"  1       1       "2.901120000000000000"  1.13767    2.03643
```