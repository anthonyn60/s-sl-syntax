# S/SL Syntax (Syntax/Semantic Language)

## Quick bits
    % This is a comment
    declaration:
    loop brackets { }
    case/if [ ]
    call statement @

## Generic Program
    input:
        %define input tokens
    output:
        %define output tokens
    type Type:
        %define type constants
    mechanism Mechanism:
        %semantic mechanism operation definitions
    rules
    RuleName:
        %actions for rules
    end
 
 ## Rules
 
 Two types of rules
 
 Procedure
 
    ruleName :
        %actions
        >> %to return
 
 Choice rule (function)
 
    ruleName >> type :
        %actions
 
To call:

    @ProcedureHeader

## Actions
Procedure call

    @
    
Procedure return
    >>
    
Recognize input token (implicit)

    identifier
    
    % assignment example
    Assign:
    pIdentifier ':=' pInteger ';';     % pColonEquals can be used in place of :=

Generate output token (emit)

    .identifier 
    
Generate error token

    #x
    % example
    #eTypeMismatch
    
Cycle

    { 
        %actions
        
        %exit, also an action, only exits innermost cycle
        >    
    }
    
Decisions and choices (if/case/switch)
Used for conditional control flow

    [ selector
        | labels:
            actions
        | labels:
            actions
        %else
        |*:
            actions
    ]

## Definitions
    input:
        pIdentifier
        pInteger
        pPlus '+' = 24 %tokens can be constrained
        pColonEquals ':='
        %etc
    output:
        sInteger
        sAdd
        sSubtract;
        %etc
    error:
        eMissingSemicolon;
        %etc
    
    %types are returned by choice rules, used to communicate with semantic mechanisms
    type Boolean:
        true
        false;
    type SymbolKind:
        syVariable
        syConstant
        syType
        syProcedure
    
## Semantic Mechanisms
- Used to manipulate values recognized by the parser and make decisions based on those stored values.
- Designed as abstract data types.
- Interface defined in S/SL, no implementation details visible at S/SL level

    mechanism MechanismIdentifier:
        oOperationIdentifier;

Two operations available

    % Update (procedural), just list the identifier
    oOperationIdentifier
    
    % Choice (functional), give return type as well
    oOperationIdentifier >> returnType
    
    % Can be parameterized by a single parameter
    oOperationIdentifier(Type)
            