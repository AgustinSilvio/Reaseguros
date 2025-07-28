# **Author:** Agustin Silvio Andrés Rojas

 agustinsilviorojas@outlook.com.ar


**Linkedin:**

<a href="https://www.linkedin.com/in/agustinsilviorojas/"><img src="https://img.shields.io/badge/linkedin-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white" /></a>&nbsp;&nbsp;&nbsp;&nbsp;


# Index

- [Index](#index)
- [Introduction](#introduction)
  - [Requirements](#requirements)
- [Ruth E. Salzmann](#ruth-e-salzmann)
- [Risk](#risk)
  - [Defining the Structure of Responsibilities](#defining-the-structure-of-responsibilities)
- [Pricing](#pricing)
- [Plot](#plot)
- [Loss Adjustment and Liability Distribution](#loss-adjustment-and-liability-distribution)
- [Bibliography](#bibliography)

# Introduction

This repository contains a visualization and contract quotation project using Python and Jupyter Notebook. The main objective is to organize the risks that accumulate, be able to price facultative reinsurance contracts, graphically see the responsibility of each participant, and settle claims based on each one's participation.


## Requirements

- Python 3.8 or superior
-  Libraries:
  - pandas
  - numpy
  - matplotlib
  - seaborn
  - datetime
  - warnings


# Ruth E. Salzmann
Salzmann tables were primarily used for rating non-proportional property excess of loss reinsurance contracts. This method, known as "exposure rating", is crucial in situations where the reinsurer does not have a sufficient credible loss history from the insured party.

Limitations and critics:
Despite their wide relevance, the Salzmann tables had significant limitations:

**Data Age:**

 The original curves were based on residential fire loss data from 1960. This means that the specific numerical values in the tables were outdated due to changes in construction practices, building codes, fire protection technologies, property values, and the overall risk landscape over time.

**Salzmann's Original Intent:**

Salzmann herself conceived her curves "only as an example" to demonstrate her methodology and explicitly "did not recommend them for further use" as universal rating tools. This indicates that their prolonged use without updates or adjustments could have led to inaccuracies.

**Difference Between Data and Application:**

The loss data used in her original analysis "differs significantly from what is typically covered by a property excess of loss reinsurance treaty."

**Dependence on External Data:**

 The exposure rating method requires the reinsurer to assign a high degree of credibility to data sources that are not their own, relying on data derived from other insurers and third-party rating systems.

**Loss Zone:**

A disadvantage of the method is that it creates a zone in each layer where losses approach, but do not reach, the next level of retention.


clasificacion_valores = {
    'B',      # (score < 60)
    'AB',    # (60 <= score < 80)  
    'A'       # (80 <= score <= 100)
}

# Risk

                                           
| Index | Start_date | Risk | Coberture | Insured_a | pure_prime_rate_per_thousand | %PML | $PML    | score_risk | MFL      | Deductible | %Retained | Type                   | Character_of_benefit         |
|-------|------------|------|-----------|-----------|-----------------------------|------|---------|------------|----------|------------|-----------|------------------------|------------------------------|
| 0     | 8/8/2025   | 1    | Building  | 3000000   | 3.0                         | 60%  | 1800000 | 100        | 3000000  | 0          | 100%      | Absolute risk coverage | Additional and independent   |
| 1     | 8/8/2025   | 1    | Content   | 12000000  | 8.0                         | 60%  | 7200000 | 100        | 12000000 | 0          | 100%      | Absolute risk coverage | Additional and independent   |
| 2     | 8/8/2025   | 1    | Machinery | 5000000   | 5.0                         | 60%  | 3000000 | 100        | 5000000  | 0          | 100%      | Absolute risk coverage | Additional and independent   |
| 3     | 8/8/2025   | 2    | Building  | 2500000   | 5.5                         | 60%  | 1500000 | 100        | 2500000  | 0          | 100%      | Absolute risk coverage | Additional and independent   |
| 4     | 8/8/2025   | 2    | Content   | 3000000   | 11.0                        | 60%  | 1800000 | 100        | 3000000  | 0          | 100%      | Absolute risk coverage | Additional and independent   |
| 5     | 8/8/2025   | 2    | Machinery | 2400000   | 6.0                         | 60%  | 1440000 | 100        | 2400000  | 0          | 100%      | Absolute risk coverage | Additional and independent   |

For this example it is 2 risks with three cobertures each one for fire insurance.

 Probable Maximum Loss (PML) refers to the greatest financial loss an insurance company is likely to face from a particular policy, assuming all protective measures—like fire suppression systems or flood defenses—function as intended. It reflects a severe but plausible scenario, not the absolute worst. This estimate is typically less than the Maximum Foreseeable Loss (MFL), which accounts for situations where those safety systems fail, leading to more extensive damage. 
 Insured_a is Insured amount.
 The type is for insurance structure. The absolute risk coverage or First-Loss is a structure where the insurer agrees to pay up to a fixed insured amount, regardless of the actual value of the loss.
 Others alternatives are for example: *Proporcionl insurance* (Indemnity based) that pays based on the actual value of the loss; *Parametric insurance* that payout when a predefined event or metric occurs.

 The character of the benefit is really important because it can determinate if diferrent coverages can be accumulative if the caracter is additional and independant. If the caracter is sustitutive from other coverage the max amount to pays is not the sum of the amounts of each coverage is the max amount of the coverages.

Example of risk clasisfication.
'B',      # (score < 60)
'AB',    # (60 <= score < 80)  
'A'       # (80 <= score <= 100)

It can used machine learning depending on the features of each risk

## Defining the Structure of Responsibilities
The structure of responsibilities in risk management is not always fixed. For instance, in reinsurance contracts based on an accident/year model, the terms and coverage can vary from one year to the next. Therefore, any framework established should be considered provisional and subject to change depending on contractual updates or evolving risk scenarios.

~~~
capasriesgo1 = {
    'Capa': ['1st Layer', '2nd Layer', '3rd Layer', '4th Layer'],
    'Desde': [0        , 1_000_000, 3_000_000, 6_000_000],
    'Hasta': [1_000_000, 3_000_000, 6_000_000, 12_000_000],
    'Aseguradora A': [0, 2_000_000, 3_000_000, 2_000_000],
    'Aseguradora B': [0, 0, 0, 3_000_000],
    'Deductible': [1_000_000, 0, 0, 0],
    'Facultative': [0, 0, 0, 1_000_000],
}

dfcapasriesgo1 = pd.DataFrame(capasriesgo1)
~~~

| Capa       | Desde    | Hasta     | Aseguradora A | Aseguradora B | Deductible | Facultativo |
|------------|----------|-----------|---------------|---------------|------------|-------------|
| 1st Layer  | 0        | 1,000,000 | 0             | 0             | 1,000,000  | 0           |
| 2nd Layer  | 1,000,000| 3,000,000 | 2,000,000     | 0             | 0          | 0           |
| 3rd Layer  | 3,000,000| 6,000,000 | 3,000,000     | 0             | 0          | 0           |
| 4th Layer  | 6,000,000|12,000,000 | 2,000,000     | 3,000,000     | 0          | 1,000,000   |

# Pricing
~~~

calculator = FacultativePremiumCalculator(df_riesgo_unificado1, df_ruth_table_numpy)

# Create diccionary of each risk with its responsability structure
risk_layers_dict = {
    1 : dfcapasriesgo1,
}

# Calculate pure premium
results = calculator.calculate_multiple_risks_premiums(risk_layers_dict)
~~~



# Plot
~~~
 plot_capas_riesgo(dfcapasriesgo1, colors, output_filename="Layers plot")
~~~
<img width="600" height="800" alt="Layers plot" src="https://github.com/user-attachments/assets/00f4e602-6e9d-4294-92c2-659df0c81bb8" />


# Loss Adjustment and Liability Distribution
The way responsibilities are defined allows for the automatic distribution of loss amounts. This allocation is typically derived from the responsibility framework—often represented in a dedicated dataframe—rather than from the detailed coverage information. However, the final result may vary due to factors such as deductibles and the specific coverage limits associated with each policy. These elements can influence how the loss is ultimately shared among the involved parties.
In the following example the loss for risk 1 is $9,500,000.00
~~~
df_result = distribute_loss_by_layers(9_500_000, dfcapasriesgo1)
print(df_result)
~~~
| Layer      | Layer Loss | Aseguradora A | Aseguradora B | Deductible | Facultative |
|------------|------------|---------------|---------------|------------|-------------|
| 1st Layer  | 1,000,000  | 0.00          | 0.0           | 1,000,000  | 0.00        |
| 2nd Layer  | 2,000,000  | 2,000,000.00  | 0.0           | 0.0        | 0.00        |
| 3rd Layer  | 3,000,000  | 3,000,000.00  | 0.0           | 0.0        | 0.00        |
| 4th Layer  | 3,500,000  | 1,166,666.67  | 1,750,000.0   | 0.0        | 583,333.33  |


# Bibliography 
 Investopedia. (n.d.). Exposure Rating: Meaning, Method, Limitations. https://www.investopedia.com/terms/e/exposure-rating.asp
 Vse.cz. (2019). Exposure curves play significant role in modelling of property perrisk excess of loss non-proportional reinsurance contracts, especially in the situations when not enough historical data is available for applying experience-based methods or if the underlying exposure changed significantly. https://pep.vse.cz/pdfs/pep/2019/02/01.pdf
 Casualty Actuarial Society. (1990). Included in the 1963 Proceedings is the paper “Rating by Layer of Insurance”, by Ruth Salsmann.  https://www.casact.org/sites/default/files/database/dpp_dpp90_90dpp395.pdf
 Boston Funeral Home. (n.d.). Obituaries: Ruth E. Salzmann.  https://www.bostonfuneralhome.net/obituaries/Ruth-E-Salzmann?obId=569247
 Casualty Actuarial Society. (2014). Ruth E. Salzmann (FCAS 1947) 1919-2014. https://www.casact.org/sites/default/files/2021-02/pubs_proceed_obituaries_salzmann_ruth-2014.pdf
 ResearchGate. (2019). Exposure Modelling in Property Reinsurance.  https://www.researchgate.net/publication/330836785_Exposure_Modelling_in_Property_Reinsurance
