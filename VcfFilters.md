# Vcf Filters

The raw output of Pindel requires further filtering to remove sequencing and mapping
artifacts.

The following described the filtering methods included in this distribution.  It
should not be considered exhaustive or perfect.

## Analysis types

There are several different types of sequencing data that can be processed through
Pindel, each have a different purpose and so the filters change depending on the
protocol employed.  For example, a WXS experiment you are unlikely
to be interested in non-exonic variants.

The categories are listed here with an abbreviation which will be used to annotate the descriptions below:

* Whole exome seq (WXS) - duplicates marked/removed (aka pulldown)
* Whole genome seq (WGS) - duplicates marked/removed
* Targeted pulldown (TG) - duplicates marked/removed
* Followup (FU) - High depth PCR based, duplicated NOT marked/removed

## Rule types

There are 2 types of rules applied during the 'flagging' step:

1. Hard rules
    * Applied as standard VCF filters and appear as FILTER entries in the header.
2. Soft Rules
    * Indicated below as 'SOFT'.
    * These use the same format as hard rules but are incorporated into the INFO
    field so they can be excluded by preference.  This provides some flexibility
    for work that may be more experimental.

The failure of some filters indicates a high likely-hood that a variant is actually
germline.  Filters of this type will be annotated with 'GERM'.

----

# Filter Rules

The filters have a numeric suffix.  In some cases values are skipped as the filter
has been retired.

Terminology:

* Wildtype = Reads/depth from 'Normal' sample.
* Mutant = Reads/depth from 'Tumour' sample.
* Reads = Number of reads exhibiting the called event.
* Depth = Number of reads ref or otherwise across this region.

## F001

Pass when more reads are found by pindel in the Mutant sample than the Wildtype.

Failure indicates likely germline call.

WXS,TG,GERM

## F002

Pass when event is <= 4bp or >4bp and no calls in wildtype.

Failure indicates likely germline call.

WXS,RG,GERM

## F003

Pass when >= 3 Mutant reads in either direction or >=2bp Mutant reads in both directions.

_F003-F005 are actually a single filter broken down into easier to manage chunks_

WXS,TG

## F004

Pass when < 10 Mutant reads.

Pass when >=10 and < 200 Mutant reads (evidence on both strands >= 0.05 total depth) or (single strand evidence >=0.08 same strand depth).

WXS,WGS,TG

_F003-F005 are actually a single filter broken down into easier to manage chunks

## F005

Pass when < 200 Mutant reads.

Pass when >= 200 Mutant reads (evidence on both strands >= 0.04 total depth) or (single strand evidence >=0.04 same strand depth).

WXS,WGS,TG

_F003-F005 are actually a single filter broken down into easier to manage chunks

## F006

Pass when event length is > 4 bp
Pass when event <= 4 bp and INFO item 'REP' <= 9.

> Here is a brief description of how the cgpPindel repeats metric works:

    catcatcatcatcatcatCATCAT
    cat cat cat cat cat cat (CAT) CAT

> The deletion CATCAT can be broken down to a single repeating tuple CAT. Directly
preceding the deletion is a 'cat' repeat which itself has 6 copies of the repeated
tuple CAT in the deletion. So in this case the repeat count would be 6.

> For this flag any variant of a length below 5 bases has this check performed over them. So:

    cattccattccattccattccattccattccattccattccattccattccattcCATTC

> with a repeat of 11 would not be filtered as the variant falls outside the minimum length.

WXS,WGS,TG

## F007

Pass when Mutant reads > 5 and Wildtype depth >= 0.08 * Mutant reads.

WXS,TG

_Specifically for high depth sequencing types, checks sufficient normal coverage_

# F008

Pass when Wiltype reads <= 0.05 * Tumour reads.

WXS,TG

# F009

Pass when in gene footprint (tabix bed input).

WXS

# F010

Pass when no overlapping records in Unmatched normal panel (tabix bed input).

WXS,WGS

# F011

N/A - legacy

# F012

Pass when event >= 11 bp.

Pass when < 11 bp and <= 9 depth on both strands.

Pass when < 11 bp and > 9 depth on both strands and event is not seen in 0.2 of wiltype or mutant reads.

WGS,GERM

# F013

N/A - legacy

# F014

N/A - legacy

# F015

Pass when no reads from combined unique BWA+Pindel count of event from Wiltype.

WGS

# F016

Pass when Mutant has > 4 reads from pindel && > 0 reads from BWA mapping.
Pass when Mutant has > 4 reads from pindel, REP=0 and pindel evidence on both strands.

WGS

# F017

Pass when variant doesn't overlap with a simple repeat.

SOFT (applied to all)

# F018

Pass when depth in original BWA mapping is >= 10 in both Mutant and Wildtype.

(unused)
