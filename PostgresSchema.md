Spaces
======

**Table** `mf_boxes`:

This table stores metadata describing sets of newspaces determined by constraints on level N, weight k, character order o, Nk^2, dimension D, and for each set lists counts of newspaces, newforms (when known), and embeddings in the set, along with a series of boolean flags indicating what data is available for newforms in the set.

Column | Type | Notes
-------|------|------
Nmin | integer | lower bound on the level N
Nmax | integer | upper bound on the level N
kmin | integer | lower bound on the weight k
kmax | integer | upper bound on the weight k
omin | integer | lower bound on the character order o
omax | integer | upper bound on the character order o
Nk2min | integer | lower bound on Nk^2
Nk2max | integer | upper bound on Nk^2
Dmin | integer | lower bound on newspace Q-dimension
Dmax | integer | upper bound on newspace Q-dimension
newspace_count | integer | total number of newspaces in this box
nonzero_newspace_count | integer | total number of nonzero newspaces in this box
newform_count | integer | total number of newforms in this box (if known, may be null)
embedding_count | bigint | total number of complex embeddings of newforms in this box
straces | boolean | set if space trace forms are stored
split | boolean | set if list of dimensions of irreducible subspaces (newforms) are stored
traces | boolean | set if newform trace forms are stored
eigenvalues | boolean | set if eigenvalue data of newforms of small dimension are stored
embeddings | boolean | set if complex embeddings are stored
lfunctions | boolean | set if lfunctions have been computed


**Table** `mf_newspaces`:

This table represents (Galois orbits of) spaces of newforms `S_k^new(N, [\chi])`, where `\chi` is a Dirichlet character of modulus N and `[\chi]` denotes its conjugacy class under the action of G_Q.  Character orbits are sorted by order and traces of values on [1..N] (lexicographically), so that 1 is the index of the orbit of the trivial character.

Column | Type | Notes
-------|------|------
label | text | (N.k.i)
level | integer | the level N of the modular form
level_radical | integer | product of prime divisors of N
level_primes | integer[] | list of primes divisors of N
level_is_prime | boolean | true if N is prime (1 is not prime)
level_is_prime_power | boolean | true if N is a prime power (1 is not a prime power, primes are prime powers)
level_is_squarefree | boolean | true if N is squarefree (1 is squarefree)
level_is_square | boolean | true if N is a square (1 is a square)
weight | smallint | the weight k of the modular form
weight_parity | smallint | (-1)^k
analytic_conductor | double precision | `N*(Exp(Psi((k)/2))/(2*pi))^2 where Psi(t) := Gamma'(t)/Gamma(t)`
Nk2 | integer | `N*k^2`
char_orbit_index | smallint | the index i of the galois orbit of the character for this space.  Galois orbits of Dirichlet characters of modulus N are sorted by the character order and then lexicographically by traces (to Q) of values on 1...N-1.  
char_orbit_label | text | string encoding the char_orbit i via 1->a, 2->b, ... 26->z, 27->ba,... (note the shift, a is 1 not 0).
conrey_indexes | integer[] | Sorted list of Conrey indexes of characters in this Galois orbit
char_order | integer | the order of the character
char_conductor | integer | Conductor of the Dirichlet character
prim_orbit_index | smallint | char_orbit for the primitive version of this character
char_degree | integer | the degree of the (cyclotomic) character field
char_parity | smallint | 1 or -1, depending on the parity of the character
char_is_real | boolean | whether the character takes only real values (trivial or quadratic)
char_values | jsonb | quadruple <N,n,u,v> where N is the level, n is the order of the character, u is a list of generators for the unit group of Z/NZ, and v is a corresponding list of integers for which chi(u[i]) = zeta_n^v[i]
sturm_bound | integer | `\floor(k*Index(Gamma0(N))/12)`
trace_bound | integer | nonnegative integer n such that the traces from 1 up to n distinguish all forms in this space (0 if space has one form, 1 if more than 1 form but dimensions are all distinct), only set when known
dim | integer | Q-dimension of the newspace `S_k^new(N,[chi])`
relative_dim | integer | Q(chi)-dimension of the newspace `S_k^new(N,[chi])`, equal to dim/degree(chi)
num_forms | smallint | number of (Hecke/Galois orbits of ) newforms, only set when known
hecke_orbit_dims | integer[] | Sorted list of Q-dimensions of Hecke orbits, only set when known
eis_dim | integer | Q-dimension of the eisenstein subspace of `M_k(N, \chi)`
eis_new_dim | integer | Q-dimension of the new eisenstein subspace of `M_k(N, \chi)`
cusp_dim | integer | Q-dimension of the cuspidal space `S_k(N, \chi)`
mf_dim | integer | Q-dimension of `M_k(N, \chi)`
mf_new_dim | integer | Q-dimension of the new subspace of `M_k(N,\chi)`
AL_dims | jsonb | For spaces with trivial character, this is a lists of triples [AL_eigs,d,n], where AL_eigs is a list of pairs [p,ev] where p is a prime dividing N and ev=+/-1 is an Atkin-Lehner eigevnalue at p, while d and n record the total dimension and number of newforms that lie in the intersection of the corresponding eigenspaces.
plus_dim | integer | For spaces with tirival character, dimension of the subspace with Fricke-eigevalue +1
trace_display | numeric[] | list of integer traces tr(a_2), tr(a_3), tr(a_5), tr(a_7), only set when dim > 0 and not yet computed in every case.
traces | numeric[] | integer coefficients a_n of the trace form (sum of all newforms in the space) for n from 1 to 1000, only set when dim > 0 and not yet computed in every case.
hecke_cutter_primes | integer[] | list of primes that appear in the hecke cutters for the newforms in this space (empty list if num_forms=1, not set for wt1 spaces or if we don't store exact eigenvalues for any forms in the space); only includes primes not dividing the level, minimal in the sense that each successive prime distinguishes forms not distinguished by any previous prime (so the length is always less than num_forms).
dihedral_dim | integer | total dimension of dihedral Hecke orbits (only set for weight 1)
a4_dim | integer | total dimension of A4 Hecke orbits (only set for weight 1)
s4_dim | integer | total dimension of S4 Hecke orbits (only set for weight 1)
a5_dim | integer | total dimension of A5 Hecke orbits (only set for weight 1)
hecke_orbit_code | bigint | Encoding of the tuple (N.k.i) into 64 bits, used as a key in mf_hecke_newspace_traces.  `N + (k<<24) + ((i-1)<<36)` this is the same as the Hecke orbit code of the first newform in the space.

**Validation** for `mf_newspaces`:

* Uniqueness constraints
  * label
  * (level, weight, char_orbit_index)
  * (level, weight, char_orbit_label)
* Overall
  * check_label : check that label matches level, weight, char_orbit_label
  * check_hecke_orbit_code : check that hecke_orbit_code matches level, weight, char_orbit_index
  * check_box_count : there should be exactly one row for every newspace in mf_boxes; for each box performing mf_newspaces.count(box query) should match newspace_count for box, and mf_newspaces.count() should be the sum of these
  * check_box_hecke_cutter_primes : check that hecke_cutter_primes is set whenever space is in a box with eigenvalues set and `min(dims) <= 20`
  * check_box_straces : check that traces, trace_bound, num_forms, and hecke_orbit_dims are set if space is in a box with straces set
  * check_char_orbit : check that char_orbit matches char_orbit_label
  * check_traces_display : check that traces_display is set whenever traces is set
  * check_traces_len : if present, check that traces has length at least 1000
  * check_trace_bound0 : check that trace_bound=0 if num_forms=1
  * check_trace_bound1 : check that trace_bound=1 if hecke_orbit_dims set and all dims distinct
  * check_trace_bound1_from_dims : check that if num_forms > 1, hecke_orbit_dims is set, and all dims distinct, then that trace_bound=1 
  * check_AL_dims_plus_dim : check that AL_dims and plus_dim is set whenever char_orbit_index=1
  * check_dim0_num_forms : check that if dim = 0 then num_forms = 0 and hecke_orbit_dims = [] (no matter what box we are in)
  * check_mf_dim : check that eis_dim + cusp_dim = mf_dim
  * check_mf_new_dim : check that eis_new_dim + dim = mf_new_dim
  * check_dim_wt1 : for k = 1 check that dim = dihedral_dim + a4_dim + a5_dim + s4_dim
  * check_relative_dim : check that char_degree * relative_dim = dim
  * check_len_hecke_orbit_dims : if present, check that len(hecke_orbit_dims) = num_forms
  * check_sum_hecke_orbit_dims : if present, check that sum(hecke_orbit_dims) = dim
  * check_sum_AL_dims : if AL_dims is set, check that AL_dims sum to dim
  * check_Nk2 : check Nk2
  * weight_parity_even, weight_parity_odd : check weight_parity
  * TODO : check that char_* atrributes and prim_orbit_index match data in char_dir_orbits table (conrey_indexes should match galois_orbit)
  * check_hecke_orbit_dims_sorted : check that hecke_orbit_dims is sorted in increasing order
* Per row
  * fast
    * check_analytic_conductor : check analytic_conductor
  * slow
    * check_level : check level_* attributes (radical, primes, is_prime,...)
    * check_sturm_bound : check that sturm_bound is exactly floor(k*Index(Gamma0(N))/12)
    * TODO : for k > 1 check that dim is the Q-dimension of S_k^new(N,chi) (using sage dimension formula)
    * TODO : for k > 1 check each of eis_dim, eis_new_dim, cusp_dim, mf_dim, mf_new_dim using Sage dimension formulas (when applicable)


**Table** `mf_gamma1`:

This table contains data for spaces of newforms `S_k^new(Gamma1(N))`, most of which is computed by summing the corresponding rows in mf_newspaces.

Column | Type | Notes
-------|------|------
label | text | (N.k)
level | integer | the level N of the modular form
level_radical | integer | product of prime divisors of N
level_primes | integer[] | list of primes divisors of N
level_is_prime | boolean | true if N is prime (1 is not prime)
level_is_prime_power | boolean | true if N is a prime power (1 is not a prime power, primes are prime powers)
level_is_squarefree | boolean | true if N is squarefree (1 is squarefree)
level_is_square | boolean | true if N is a square (1 is a square)
weight | smallint | the weight k of the modular form
weight_parity | smallint | (-1)^k
analytic_conductor | double precision | `N*(Exp(Psi((k)/2))/(2*pi))^2 where Psi(t) := Gamma'(t)/Gamma(t)`
Nk2 | integer | `N*k^2`
sturm_bound | integer | `floor(k*Index(Gamma1(N))/12)`
trace_bound | integer | nonnegative integer n such that the traces from 1 up to n distinguish all forms in this space (0 if space has 1 form, 1 if more than 1 form but dimensions are all distinct)
dim | integer | Q-dimension of S_k^new(Gamma1(N))
num_forms | integer | number of (Hecke/Galois orbits of) newforms, only set when known
hecke_orbit_dims | integer[] | Sorted list of Q-dimensions of Hecke orbits, only set when known
num_spaces | integer | number of nozero newspaces `S_k^new(N,[\chi])` in `S_k^{new}(Gamma1(N))`
newspace_dims | integer[] | list of Q-dimensions of newspaces `S_k^new(N,\chi)` in `S_k^new(Gamma1(N))` ordered by character orbit index
eis_dim | integer | Q-dimension of the eisenstein subspace of `M_k(Gamma1(N))`
eis_new_dim | integer | Q-dimension of the new eisenstein subspace of`M_k(Gamma1(N))`
cusp_dim | integer | Q-dimension of the cuspidal space `S_k(Gamma1(N))`
mf_dim | integer | Q-dimension of the full space`M_k(Gamma1(N))`
mf_new_dim | integer | Q-dimension of the new subspace of `M_k(N,\chi)`
trace_display | numeric[] | list of integer traces tr(a_2), tr(a_3), tr(a_5), tr(a_7), only set when dim > 0, not yet computed in every case.
traces | numeric[] | integer coefficients a_n of the trace form (sum of all newforms) for n from 1 to 1000, only set when dim > 0, not yet computed in every case.
dihedral_dim | integer | total dimension of dihedral Hecke orbits (only set for weight 1)
a4_dim | integer | total dimension of A4 Hecke orbits (only set for weight 1)
s4_dim | integer | total dimension of S4 Hecke orbits (only set for weight 1)
a5_dim | integer | total dimension of A5 Hecke orbits (only set for weight 1)

**Validation** for `mf_gamma1`:

* Uniqueness
  * label
  * (level, weight)
* Overall
  * check_label : check that label matches level and weight
  * check_box_count : there should be a row present for every pair (N,k) satisfying a box constraint on N,k,Nk2
  * check_box_traces : check that traces is set if space is in a box with traces set and no dimension constraint
  * check_dim_wt1 : for k = 1 check that dim = dihedral_dim + a4_dim + a5_dim + s4_dim
  * check_trace_display : check that trace_display is present whenever traces is
  * check_traces_len : if present, check that traces has length at least 1000
  * check_mf_dim : check that eis_dim + cusp_dim = mf_dim
  * check_dim : check that eis_new_dim + mf_new_dim = dim
  * check_Nk2 : check Nk2
  * weight_parity_even, weight_parity_odd : check weight_parity
  * check_newspaces_numforms : if num_forms is set verify that it is equal to the sum of num_forms over newspaces with matching level and weight
  * check_newspaces_hecke_orbit_dims : if hecke_orbit_dims is set, verify that it is equal to the (sorted) concatenation of hecke_orbit_dims over newspaces with matching level and weight
  * check_newspaces_newspace_dims : check that newspace_dims is equal to the (sorted) concatenation of dim over newspaces with this level and weight
  * check_newspaces_num_spaces : check that num_spaces matches number of char_orbits of level N and number of records in mf_newspaces with this level and weight
* Per row
  * local
    * check_level : check level_* attributes
    * check_analytic_conductor : check analytic_conductor
    * check_sturm_bound : check that sturm_bound is exactly floor(k*Index(Gamma1(N))/12)
    * check_Sk_dim_formula : check that dim = dim S_k^new(Gamma1(N))
    * TODO : for k > 1 check eis_dim, eis_new_dim, cusp_dim, mf_dim, mf_new_dim using Sage dimension formulas

**Table** `mf_newspace_portraits`:

Column | Type | Notes
-------|------|------
label | text | label (N.k.i) of the newspace
level | integer | level N
weight | smallint | weight k
char_orbit_index | smallint | index of the character orbit `[\chi]` n the sorted list of character orbits of modulus N
portrait | text | base-64 encoded image of the newspace (plot created by portrait.sage) to display in the properties box

**Validation** for `mf_newspace_portraits`:

* Uniqueness
  * label
  * level,weight,char_orbit_index
* Overall
  * check_label : check that label matches level,weight,char_orbit_label
  * check_portraits_count : check that there is a portrait present for every nonempty newspace in box where straces is set (attached to mf_newspace_portraits)


**Table**`mf_gamma1_portraits`:

Column | Type | Notes
-------|------|------
label | text | label N.k for the space `S_k^new(Gamma1(N))`
level | integer | level N
weight | smallint | weight k
portrait | text | base-64 encoded image of the newspace (plot created by portrait.sage) to display in the properties box

**Validataion** for `mf_gamma1_portraits`:

* Uniqueness
  * label
  * level, weight
* Overall
  * check_label : check that label matches level, weight
  * check_portraits_count : check that there is a portrait present for every record in mf_gamma1 with `dim > 0` and `level <= 4000` (attached to mf_gamma1)

**Table** `mf_subspaces`:

This table represents embeddings of newspaces at level M into cusp spaces at level N (these will be old at level N except when M=N).

Column | Type | Notes
-------|------|------
label | text | label N.k.i for the cuspidal space `S_k(N, [\chi])` (same as the label for `S_k^{new}(N, [\chi])`)
level | integer | level N of the cuspidal space `S_k(N, [\chi])`
weight | smallint | weight k of the cuspidal space `S_k(N, [\chi])`
char_orbit_index | smallint | index i of the character orbit `[\chi]` in the sorted list of character orbits of modulus N
char_orbit_label | text | base-26 encoding (1='a') of index i of the character orbit that appears in label
conrey_indexes | integer[] | list of Conrey indexes n of the characters N.n in the Galois orbit indexed by i
sub_label | text | The label of the newspace `S_k^{new}(M, [\psi])` that appears as a non-trivial subspace of`S_k(N, [\chi])`
sub_level | integer | (M)
sub_char_orbit_index | smallint | index j of `[\psi]` in sorted list of character orbits of modulus M
sub_char_orbit_label | text | base-26 encoding (1='a') of index j of the subspace character orbit that appears in sub_label
sub_conrey_indexes | integer[] | list of Conrey indexes n of the characters M.n in the Galois orrbit indexed by j.
sub_dim | integer | the dimension of `S_k^{new}(M, [\psi])`
sub_mult | integer | Multiplicity of`S_k^{new}(M, [\psi])` as a direct summand of `S_k(N, [\chi])` (this is just the number of divisors of N/M).  Summing dimensions of embedded newspaces with multiplicity gives the dimension of the cusp space.

**Validation** for `mf_subspaces`:

* Unique
  * (label,sub_label)
* Overall
  * check_sub_mul_positive : sub_mult is positive
  * check_label : check that label matches level, weight char_orbit_label
  * check_char_orbit_label : check that char_orbit_label matches level, char_orbit_index
  * check_sub_label : check that sub_label matches sub_level, weight, sub_char_orbit_index
  * check_level_divides : check that sub_level divides level
  * check_sub_char_orbit_label : check that sub_char_orbit_label matches sub_level, sub_char_orbit_index
  * check_conrey_indexes : check that conrey_indexes matches galois_orbit for char_orbit_label in char_dir_orbits
  * check_sub_conrey_indexes : check that sub_conrey_indexes matches galois_orbit for sub_char_orbit_label in char_dir_orbits
  * check_sub_dim : check that sub_dim = dim S_k^new(sub_level, sub_chi)
  * check_oldspace_decomposition_totaldim : check that summing sub_dim * sub_mult over rows with a given label gives dim of S_k^old(N,chi) (attached to mf_newspaces)

**Table** `mf_gamma1_subspaces`:

Column | Type | Notes
-------|------|------
label | text | label N.k for the cuspidal space `S_k(Gamma1(N))` (same as the label for `S_k^{new}(Gamma1(N))`
level | integer | level N of the space S_k(Gamma_1(N))
weight | smallint | weight k of the space S_k(Gamma_1(N))
sub_level | integer | level M of the newspace S_k^{new}(Gamma_1(M)) that embed in S^k(Gamma_1(N))
sub_dim | integer | dimension of S_k^{new}(Gamma_1(M))
sub_mult | integer | multiplicity of S_k^{new}(Gamma_1(M)) as a direct summand of S_k^{Gamma_1(N)).  Summing dimensions of embedded newspaces S_k^{new}(Gamma_1(M)) with multiplicity gives the dimension of the cusp space S_k(Gamma_1(N).

**Validation** for `mf_gamma1_subspaces`:

* Unique
  * (label, sub_level)
* Overall
  * check_label : check that label matches level, weight
  * check_level_divides : check that sub_level divides level
  * check_sub_dim : check that sub_dim = dim S_k^new(Gamma1(sub_level)) and is positive
  * check_oldspace_decomposition_totaldim : check that summing sub_dim * sub_mult over rows with same label gives dim S_k^old(Gamma1(N)) (attached to mf_gamma1)

Newforms
========

**Table** `mf_newforms`:

Column | Type | Notes
-------|------|------
label |  text | (N.k.i.x)
space_label | text | (N.k.i)
level | integer | the level N of the modular form
level_radical | integer | product of prime divisors of N
level_primes | integer[] | list of prime divisors of N
level_is_prime | boolean | true if N is prime (1 is not prime)
level_is_prime_power | boolean | true if N is a prime power (1 is not a prime power, primes are prime powers)
level_is_squarefree | boolean | true if N is squarefree (1 is squarefree)
level_is_square | boolean | true if N is a square (1 is a square)
weight | smallint | the weight k of the modular form
weight_parity | smallint | (-1)^k
analytic_conductor | double precision | `N*(Exp(Psi((k)/2))/(2*pi))^2 where Psi(t) := Gamma'(t)/Gamma(t)`
Nk2 | integer | `N*k^2`
char_orbit_index | smallint | The index i of the Galois orbit of this form in the sorted list of character orbits, as described above.
char_orbit_label | text | string encoding i (with a=1).
char_conductor | integer | Conductor of the Dirichlet character
prim_orbit_index | smallint | char_orbit for the primitive version of this character
char_order | integer | the order of the character
conrey_indexes | integer[] | Sorted list of Conrey indexes of characters in this Galois orbit
char_degree | integer | Degree of the (cyclotomic) character field
char_parity | smallint | 1 or -1, depending on the parity of the character
char_is_real | boolean | whether the character takes only real values (trivial or quadratic)
char_values | jsonb | quadruple <N,n,u,v> where N is the level, n is the order of the character, u is a list of generators for the unit group of Z/NZ, and v is a corresponding list of integers for which chi(u[i]) = zeta_n^v[i]
hecke_orbit | integer | (X) An integer that is encoded into x in the label via 1=a, 2=b, 26=z, 27=ba, 28=bb.  Note the shift: the letter is the Cremona code for X-1.
hecke_orbit_code | bigint | encoding of the tuple (N.k.i.x) into 64 bits, used in eigenvalue tables.  `N + (k<<24) + ((i-1)<<36) + ((x-1)<<52)`.
dim | integer | the Q-dimension of this Galois orbit
relative_dim | integer | the Q(chi)-dimension of this Hecke orbit (=dim/char_degree)
field_disc | numeric | discriminant of the coefficient field, if known
field_disc_factorization | numeric[] | factorization of field discriminant stored as ordered list of pairs [p,e]
field_poly | numeric[] | list of integers giving defining polynomial for the Hecke field (standard Sage order of coefficients)
field_poly_is_cyclotomic | boolean | true if field_poly is a cylcotomic polynomial (the field might be Q(zeta_n) even when this flage is not set if we haven't chosen a cyclotomic polynomial to define it)
field_poly_is_real_cyclotomic | boolean | true if field_poly is the minimal polynomial of zeta_n + zeta_n^-1 for some n (the field might be `Q(zeta_n)^+` even when this flage is not set if we haven't chosen a cyclotomic polynomial to define it)
field_poly_root_of_unity | integer | the value of n if either field_poly_is_cylotomic of field_poly_is_real_cyclotomic is set
is_polredabs | boolean | whether the polynomial has been reduced by Pari's `polredabs`
nf_label | text | LMFDB label for the corresponding number field (can be NULL)
is_self_dual | boolean | true if L-func is self-dual (coeff field is totally real)
hecke_ring_generator_nbound | integer | minimal integer m such that a_1,...,a_m generate the Hecke ring
hecke_ring_index | numeric | (a divisor of) the index of the order generated by the Hecke eigenvalues in the maximal order.
hecke_ring_index_factorization | numeric[] | Factorization of hecke_ring_index stored as ordered list of pairs [p,e].
hecke_ring_index_proved | boolean | whether the index has been proved correct (computing the maximal order may not be possible)
trace_hash | bigint | linear combination of the a_p between 2^12 and 2^13 reduced mod 2^61-1 as defined in BSSVY, only guaranteed for `wt > 1` and `dim <= 20`
trace_zratio | double precision | proportion of zero a_p values for `p <= 2^13` (rounded to three decimal places)
trace_moments | numeric[] | list of moments of a_p/p^((k-1)/2) computed over `p <= 2^13` (rounded to three decimal places)
related_objects | text[] | list of text URLs of related objects (e.g. elliptic curve isogeny class, Artin rep, ...), e.g. ["EllipticCurve/Q/11/a"]
analytic_rank | smallint | order of vanishing of L-function at s=1 (an upper bound, tight if analytic_rank_proved is set)
analytic_rank_proved | boolean | true if analytic rank is provably correct (it is always an upper bound)
self_twist_type | smallint | 0=none, 1=cm, 2=rm, 3=both
is_self_twist | boolean | whether this form is a self twist
is_cm | boolean | whether the form has CM
is_rm | boolean | whether the form has RM
self_twist_discs | integer[] | list of discriminants giving self twists (either 0,1,or 3 quadratic discriminants)
cm_discs | integer[] | list of CM discriminants (the negative discriminants listed in self_twist_discs)
rm_discs | integer[] | list of RM discriminants (the positive discriminants listed in self_twist_discs)
self_twist_proved | boolean | whether the self twists have been proved unconditionally
has_non_self_twist | smallint | 1 if form admits a non-trivial inner twist, 0 if it does not, -1 if unknown
inner_twists | integer[] | List of septuples of integers [b,m,M,o,parity,order,disc] where <M,o> identifies the Galois orbit if a Dirichlet character, m is the number of characters in this orbit that give rise to an inner twist, and b is 1 if the inner twists is proved.  All inner twists are guaranteed to be included in the list, but those without proved set could be false positives.
inner_twist_count | integer | number of inner twists (includes proved and unproved), -1 if inner twists have not been computed (this applies to all forms of dimension > 20 and weight > 1)
atkin_lehner_eigenvals | integer[] | a list of pairs [p, ev] where ev is 1 or -1, the Atkin-Lehner eigenvalue for each p dividing N (NULL overall if nontrivial character, an empty list for level 1 and trivial character)
atkin_lehner_string | text | list of signs +/- of Atkin-Lehner eigenvalues ordered by p (facilitates lookups)
fricke_eigenval | smallint | product of the Atkin-Lehner eigenvalues (NULL if nontrivial character)
hecke_cutters | jsonb | a list of pairs [p, F_p] where F_p is a list of integers encoding a polynomial; the intersection of the kernels of F_p(T_p) is this Hecke orbit
qexp_display | text | latexed string for display on search page results
trace_display | numeric[] | list of traces tr(a_2), tr(a_3), tr(a_5), tr(a_7) for display on search page results
traces | numeric[] | full list of traces tr(a_n) for n from 1 to 1000 (or more)
projective_image_type | text | for weight 1 forms, one of "Dn", "A4", "S4", "A5"
projective_image | text | for weight 1 forms, isomorphism class of project image (e.g. which Dn)
projective_field | numeric[] | for weight 1 forms, list of integer coefficients of polynomial whose splitting field is the fixed field of the kernel of the projective Galois rep (subfield of the artin field fixed be the center of its Galois group)
projective_field_label | text | LMFDB label of projective field (if present)
artin_degree | integer | for weight 1 forms, order of the image of the Galois rep, equivalently, the degree of the Artin field
artin_image | text | for weight 1 forms, small group label of the image of the Galois rep (and the Galois group of the artin field)
artin_field | numeric[] | for weight 1 forms, list of integer coefficients of polynomial whose splitting field is the fixed field of the Galois rep (equivalently, a defining polynomial for the 2-dim Artin rep corresponding to this weight 1 form)
artin_field_label | text | LMFDB label of artin field (if present)
sato_tate_group | text | LMFDB label of Sato-Tate group (currently only present for weight k > 1)

**Validation** for `mf_newforms`:

* Unique
  * label
  * level, weight, char_orbit_index, hecke_orbit
  * hecke_orbit_code
* Overall
  * check_label : check that label matches level, weight, char_orbit_index, hecke_orbit
  * check_hecke_orbit_code : check that hecke_orbit_code matches label
  * check_box_count : there should be exactly one row for every newform in a box listed in mf_boxes with newform_count set; for each such box performing mf_newforms.count(box query) should match newform_count for box, and mf_newforms.count() should be the sum of these
  * check_hecke_ring_generator_nbound : hecke_ring_generator_nbound > 0
  * check_space_label : check that space_label matches level, weight, char_orbit_index
  * check_relative_dim : check that char_degree * relative_dim = dim
  * check_newspaces_overlap : check that all columns mf_newforms has in common with mf_newspaces other than label, dim, relative_dim match (this covers all atributes that depend only on level, weight, char) (this implies) check that space_label is present in mf_newspaces
  * check_polredabs_set : check that if nf_label is set, then is_polredabs is true
  * check_field_poly_consequences : if field_poly is set, check that is_polredabs, hecke_ring_generator_nbound, and qexp_display are set
  * check_field_poly : if field_poly is set, check that is monic and of degree dim
  * check_traces_length : check that traces is present and has length at least 10000
  * check_trace_display : check that trace_display is present and has length at least 4
  * check_number_field : if nf_label is present, check that there is a record in nf_fields and that mf_newforms field_poly matches nf_fields coeffs, and check that is_self_dual agrees with signature, and field_poly_disc agrees with disc_sign * disc_abs in nf_fields
  * check_field_poly_disc : if hecke_ring_index_proved is set, verify that field_poly_disc is set
  * FIXME check_analytic_rank_proved : check that analytic_rank_proved is True  when analytic_rank is set (log warning if not)
  * check_self_twist_type : check that self_twist_type is in {0,1,2,3} and matches is_cm and is_rm
  * check_cmrm_discs : check that self_twist_discs is consistent with self_twist_type (e.g. if self_twist_type is 3, there should be 3 self_twist_discs, one pos, two neg)
  * check_self_twist_discs : check that cm_discs and rm_discs have correct signs and that their union is self_twist_discs
  * check_self_twist_proved : check that self_twist_proved is set (log warning if not, currently there are 10-20 where it is not set)
  * check_fricke_eigenval : if present, check that fricke_eigenval is product of atkin_lehner_eigenvals
  * check_sato_tate_set : for k>1 check that sato_tate_group is set
  * check_sato_tate_value : for k>1 check that sato_tate_group is consistent with is_cm and char_order (it should be 1.2.3.cn where n=char_order if is_cm is false, and 1.2.1.dn if is_cm is true)
  * check_projective_image_type : for k=1 check that projective_image_type is present,
  * check_projective_image : if present, check that projective_image is consistent with projective_image_type
  * check_projective_field : if present, check that projective_field_label identifies a number field in nf_fields with coeffs = projective_field
  * check_artin_field : if present, check that artin_field_label identifies a number field in nf_fields with coeffs = artin_field
  * check_artin_degree : if present, check that artin_field has degree equal to artin_degree
  * check_trivial_character_cols : check that atkin_lehner_eigenvalues, atkin_lehner_string, and fricke_eigenval are present if and only if char_orbit_index=1 (trivial character)
  * check_inner_twists : check that inner_twists is consistent with inner_twist_count and that both are present if field_poly is set
  * check_has_non_self_twist : check that has_non_self_twist is consistent with inner_twist_count and self_twist_type
  * check_field_disc_factorization : if present, check that field_disc_factorization matches field_disc
  * check_hecke_ring_index_factorization : if present, verify that hecke_ring_index_factorization matches hecke_ring_index
  * check_analytic_rank_set : if newform is in a box with lfunctions set, check that analytic_rank
  * check_analytic_rank : if analytic_rank is present, check that matches order_of_vanishing in lfunctions record, and is are constant across the orbit
* Per row
  * local
    * check_projective_field_degree : if present, check that projective_field has degree matching projective_image (4 for A4,S4, 5 for A5, 2n for Dn)
  * slow
    * check_self_twist_disc : check that self_twist_disc = [elt[6] for elt in inner_twists if elt[6] is not None]
    * check_field_poly_properties : if present, check that field_poly is irreducible, if field_poly_is_cyclotomic   are set, verify this
    * TODO : for each discriminant D in self_twist_discs, check that for each prime p not dividing the level for which (D/p) = -1, check that traces[p] = 0 (we could also check values in mf_hecke_nf and/or mf_hecke_cc, but this would be far more costly)
    * check_related_objects : check that URLS in related_objects are valid and identify objects present in the LMFDB
    * check_related_objects : if related_objects contains an Artin rep, check that k=1 and that conductor of artin rep matches level N
    * TODO : if k=2, char_orbit_index=1 and dim=1 check that elliptic curve isogeny class of conductor N is present in related_objects (add this one to the above)
  * extra slow
    * TODO : if nf_label is not present and field_poly is present, check whether is_self_dual is correct (if feasible)
    * TODO : if is_self_dual is present but field_poly is not present, check that embedding data in mf_hecke_cc is consistent with is_self_dual and/or check that the lfunction self_dual attribute is consistent
    * TODO : if present, check that artin_image is consistent with artin_degree and projective_image (quotient of artin_image by its center should give projective_image) (use GAP)
  * newspace
    * TODO : check that dim is present in hecke_orbit_dims array in newspace record and that summing dim over rows with the same space label gives newspace dim
  * char_dir_orbits
    * TODO : check that each level M in inner twists divides the level and that M.o identifies a character orbit in char_dir_orbits with the listed parity
    * TODO : if field_poly_is_real_cycolotomic is set, verify this (precompute table)


**Table** `mf_newform_portraits`:

Column | Type | Notes
-------|------|------
label | text | label (N.k.i.x) of the newform
level | integer | level N
weight | smallint | weight k
char_orbit_index | smallint | character orbit index i
hecke_orbit | integer | Hecke orbit index x
portrait | text | base-64 encoded image of the newform (plot created by portrait.sage) to display in the properties box

**Validation** for `mf_newform_portraits`:

* check_label : check that label matches level, weight, char_orbit_index, hecke_orbit
* check_portraits : check that there is exactly one record in mf_newform_portraits for each record in mf_newforms, uniquely identified by label (attached to mf_newforms)

Hecke eigenvalues
=================

**Table** `mf_hecke_nf`:

Column | Type | Notes
-------|------|------
label | text | label of modular form (N.k.i.x)
level | integer | level N
weight | smallint | weight k
char_orbit_index | smallint | character orbit index i
hecke_orbit_code | bigint | encoding of the tuple (N.k.i.x) into 64 bits
field_poly | numeric[] | list of integers of defining polynomial for Hecke field
hecke_ring_rank | integer | rank of Hecke ring as a free Z-module (same as dimension of form, degree of field_poly)
hecke_ring_power_basis | boolean | if true the chanage of basis matrix is the (implicit) identity matrix, in which case hecke_ring_numerators, ..., hecke_ring_inverse_denominators are set to null
hecke_ring_cyclotomic_generator | integer | either zero or an integer m such that the an and ap are encoded as sparse integer polynomials in zeta_m (typically same as field_poly_root_of_unity but this is not required)
hecke_ring_numerators | numeric[] | List of lists of integers, giving the numerators of a basis for the Hecke order in terms of the field generator specified by the field polynomial
hecke_ring_denominators | numeric[] | List of integers, giving the denominators of the basis
hecke_ring_inverse_numerators | numeric[] | List of lists of integers, giving the numerators of the inverse basis that specifies powers of nu in terms of the betas
hecke_ring_inverse_denominators | numeric[] | List of integers, giving the denominators of the inverse basis
hecke_ring_character_values | jsonb | list of pairs [[m1,[a11,...a1n]],[m2,[a12,...,a2n]],...] where [m1,m2,...,mr] are generators for Z/NZ and [ai1,...,ain] is the value of chi(mi) expressed in terms of the Hecke ring basis or in cyclotomic representation [[c,e]] encoding c x zeta_m^e where m is hecke_ring_cyclotomic_generator
an | jsonb | list of a1,...,a100, where each an is either a list of integers encoding an in the Hecke ring basis described above or a list of pairs [[c1,e1],...,[cr,er]] encoding an = c1 x zeta_m^e1 + ... + cr x zeta_m^er, where m is the value of hecke_ring_cyclotomic_generator (if nonzero)
ap | jsonb | list of lists of integers expressing a_p for p=2,3,5,...,pmax in same format as an
maxp | integer | largest prime p for which ap is stored

**Validation** for `mf_hecke_nf`:

* Overall
  * check_bijection : there should be a record present for every record in mf_newforms that has field_poly set (and no others, check count)
  * check_hecke_orbit_code : check that label matches hecke_orbit_code and is present in mf_newforms
  * check_field_poly : check that field_poly matches field_poly in mf_newforms
  * check_hecke_ring_rank : check that hecke_ring_rank = deg(field_poly)
  * check_hecke_ring_power_basis_set: if hecke_ring_power_basis is set, check that hecke_ring_cyclotomic_generator is 0 and hecke_ring_numerators, ... are null
  * check_hecke_ring_cyclotomic_generator : if hecke_ring_cyclotomic_generator is greater than 0 check that hecke_ring_power_basis is false and hecke_ring_numerators, ... are null
  * check_field_poly_is_cyclotomic : if hecke_ring_cyclotomic_generator > 0, check that field_poly_is_cyclotomic is set in mf_newforms record.
  * check_maxp : check that maxp is at least 997
* Slow
  * check_hecke_ring_character_values_and_an : check that hecke_ring_character_values has the correct format, depending on whether hecke_ring_cyclotomic_generator is set or not
  * check_hecke_ring_character_values_and_an : check that an has length 100 and that each entry is either a list of integers of length hecke_ring_rank (if hecke_ring_cyclotomic_generator=0) or a list of pairs
  * check_hecke_ring_character_values_and_an : check that ap has length pi(maxp) and that each entry is formatted correctly (as for an)

**Table** `mf_hecke_traces`:

Column | Type | Notes
-------|------|------
hecke_orbit_code | bigint | encoding of the tuple (N.k.i.x) into 64 bits
n | integer | index of a_n
trace_an | numeric | trace of a_n down to Z

**Validation** for `mf_hecke_traces`:

* Unique
  * (hecke_orbit_code,n)
* Overall
  * check_traces_count : there should be exactly 1000 records present for each record in mf_newforms (attached to mf_newforms)
  * check_hecke_orbit_code_newforms : check that hecke_orbit_code is present in mf_newforms (via checking surjectivity and fiber size)
  * check_traces_match : check that trace_an matches traces[n] in mf_newforms record

**Table** `mf_hecke_lpolys`:

Column | Type | Notes
-------|------|------
hecke_orbit_code | bigint | encoding of the tuple (N.k.i.x) into 64 bits
p | integer | prime identifying L-poly L_p(T) = prod_(sigma in Gal(Q(f)/Q) (1 - sigma(a_p(f))T + chi(p)p^(k-1)T^2))
lpoly | numeric[] | integer coefficients of L_p(t) (total of 2 * dim + 1 coeffs at good p, either 1 or dim+1 at bad p)

**Validation** for `mf_hecke_lpolys`:

* Unique
  * (hecke_orbit_code,p)
* Overall
  * check_lpoly_count : there should be exactly 25 records present for each recod in mf_newforms with field_poly set (attached to mf_newforms)
  * check_total_count : total = 25 * db.mf_newforms.count({'field_poly':{'$exists':True}})
  * check_prime_count : check that every prime p less than 100 occurs exactly once for each hecke_orbit_code
  * check_hecke_orbit_code : check that hecke_orbit_code is present in mf_newforms
  * check_lpoly : check that degree of lpoly is twice the dimension in mf_newforms for good primes
  * check_lpoly : check that linear coefficient of lpoly is `-trace(a_p)` and constant coefficient is 1

**Table** `mf_hecke_newspace_traces`:

Column | Type | Notes
-------|------|------
hecke_orbit_code | bigint | encoding of the tuple (N.k.i) into 64 bits (this is the same as the Hecke orbit code for the first newform in the space, but in this table the traces are sums over the entire newspace N.k.i)
n | integer | index of a_n
trace_an | numeric | trace of a_n down to Z, where a_n is the sum of a_n over all newforms in the space

**Validation** for `mf_hecke_newspace_traces`:

* Unique
  * (hecke_orbit_code,n)
* Overall
  * check_traces_count : there should be exactly 1000 records present for each record in mf_newfspaces in a box that has straces set (attached to mf_newspaces)
  * check_traces_match : check that trace_an matches traces[n] in mf_newspaces record (attached to mf_newspaces)
  * check that hecke_orbit_code is present in mf_newspaces

**Table** `mf_hecke_cc`:

Column | Type | Notes
-------|------|------
hecke_orbit_code | bigint | encoding of the tuple (N.k.i.x) into 64 bits
lfunction_label | text | (N.k.c.x.n) where N.c is the Conrey label of the restriction to the cyclotomic field and n enumerates the embeddings with the same character (starting at 1)
conrey_label | integer | the Conrey label for the restriction of the embedding to the character field
embedding_index | integer | enumeration of which embedding (shows up in L-function link) for the given conrey label
embedding_m | integer | enumeration of which embedding over all conrey labels in the specified hecke orbit.  Ordering is the same as lexicographic on (conrey_label, embedding_index).  1-indexed.
embedding_root_real | double precision | real part of the root corresponding to this embedding
embedding_root_imag | double precision | imaginary part of the root corresponding to this embedding
an_normalized | double precision[] | list of pairs {x,y} of doubles x, y so that `a_n = n^{k-1)/2} * (x + iy)` for `n \in [1,1000]`
angles | double precision[] | list of `\theta_p`, where '\theta_p' is `Null` if `p` is bad, and for good `p` we have `a_p = p^{(k-1)/2} (e^{2\pi i \theta_p} + chi(p)e^{-2\pi i \theta_p})`; indexed by increasing primes p less than 1000, where theta_p lie in (-0.5,0.5]. Furthermore, we store the minimum value of the two options for `\theta_p` in that interval.

**Validation** for `mf_hecke_cc`:

* Unique
  * lfunction_label
* Overall
  * check_embeddings_count : there should be a record present for every record in mf_newforms that lies in a box weight embeddings set (currently this is all of them) (attached to mf_newforms)
  * check_hecke_orbit_code_newforms : check that hecke_orbit_code is present in mf_newforms
  * check_lfunction_label : check that lfunction_label is consistent with hecke_orbit_code, conrey_label, and embedding_index
  * check_an_length : check that an_normalized is a list of pairs of doubles of length at least 1000
  * check_embeddings_count : check that for such box with embeddings set, the number of rows in mf_hecke_cc per hecke_orbit_code matches dim (attached to mf_newforms)
  * check_embeddings_count_boxcheck : check that for such box with embeddings set, that summing over `dim` matches embeddings_count (attached to mf_newforms)
  * check_roots : check that embedding_root_real, and embedding_root_image  present in mf_hecke_cc whenever field_poly is present (attached to mf_newforms)
  * check_embedding_index : check that embedding_index is consistent with conrey_label and embedding_m
  * check_embedding_m : check that embedding_m is consistent with conrey_label and embedding_index
  * check_conrey_indexes : when grouped by hecke_orbit_code, check that conrey_labels match conrey_indexes,  embedding_index ranges from 1 to relative_dim (when grouped by conrey_label), and embedding_m ranges from 1 to dim

* Per row
  * check_angles : check that angles lie in (-0.5,0.5] and are null for p dividing the level
  * check_roots_are_roots : check that embedding_root_real, and embedding_root_image  approximate a root of field_poly (attached to mf_newforms)
  * TODO : (optional) check that summing (normalized) an over embeddings with a given hecke_orbit_code gives an approximation to tr(a_n)*n^{k-1}/2 -- we probably only want to do this for specified newforms/newspaces, otherwise this will take a very long time.

Dirichlet characters
====================

**Table** `char_dir_orbits`:

Column | Type | Notes
-------|------|------
orbit_label | text | (N.i), where N is the modulus and i is the orbit index
orbit_index | smallint | (i) Index in the list of traces down to Q of the values of all characters of modulus N
modulus | integer
conductor | integer
prim_orbit_index | smallint | Orbit index for the primitive character inducing this one (note that this index identifies a Galois orbit of characters of modulus M = conductor)
order | integer
parity | smallint
galois_orbit | integer[] | sorted list of conrey_labels in the same galois orbit
is_real | boolean | if quadratic or trivial
is_primitive | boolean | if modulus = conductor
char_degree | integer | degree of the cyclotomic field containing the image, ie Euler phi of the order; this is the same as the size of the Galois orbit

**Validation** for `char_dir_orbits`:

* Unique
  * orbit_label
* Overall
  * check_total_count : there should be a record present for every character orbit of modulus up to 10,000 (there are 768,512)
  * check_trivial : check that orbit_index=1 if and only if order=1
  * check_conductor_divides : check that conductor divides modulus
  * check_primitive : check that orbit specified by conductor, prim_orbit_index is present
  * check_is_real : check that is_real is true if and only if `order <= 2`
  * check_galois_orbit_len : check that char_degee = len(Galois_orbit)
  * FIXME : check that is_primitive is true if and only if modulus=conductor
* Fast
  * check_char_degree : check that char_degree = euler_phi(order)
* Slow
  * TODO : check order and parity by constructing a Conrey character in Sage (use the first index in galois_orbit)

**Table** `char_dir_values`:

Note that the values in this table are stored as integers m so that the actual value is `e^{2\pi i m/d}` where `d` is the `order`.

Column | Type | Notes
-------|------|------
label | text | N.n where N is the modulus and n is the conrey index
modulus | smallint | N
conrey_index | smallint | n
orbit_label | text | N.i where N is the modulus and i is the orbit_index
prim_label | text | the label of primitive character inducing this one
modulus | integer
conrey_index | integer
order | integer
values | integers[] | list of pairs [x,m] giving first twelve values e(m/n) on x=-1,1, then the next ten integers relatively prime to the modulus, where n is the order of the character
values_gens | integers[] | list of pairs [x, m] giving values on generators x of the unit group

**Validation** for `char_dir_values`

* Unique
  * label
* Overall
  * check_total_count : The number of entries in char_dir_values matching a given orbit_label should be char_degree (checked in char_dir_values)
  * check_order_match : order should match order in char_dir_orbits for this orbit_label
* Fast
  * check_galois_orbit : Conrey index n in label should appear in galois_orbit for record in char_dir_orbits with this orbit_label (attached to char_dir_orbits)
  * FIXME : The x's listed in values and values_gens should be coprime to the modulus N in the label
  * check_parity : the value on -1 should agree with the parity for this char_orbit_index in char_dir_orbits (attached to char_dir_orbits)
  * FIXME : for x's that appear in both values and values_gens, the value should be the same.

