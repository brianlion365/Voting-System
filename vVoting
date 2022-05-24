// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
/// @title Voting with delegation.
contract Ballot {
 
   struct Voter {

       uint weight; //weight is accumlated by delegation 
       bool voted; //Boolean is a true or false number // this decides if the person already voted 
       address delegate; //the person the vote is delegated to 
       uint vote; //the vote will be in a intenger  

   }


    struct Proposal {

        bytes32 name;  //short name up to 32 bites 
        uint voteCount; //which will count the vote



    }


    address public chairperson; 

    //this declares the state variable that 
    //Stores the voter strcut for each possible address 



    mapping(address => Voter) public voters;


    // A dyanamically sized array of 'proposal struts 


    Proposal[] public proposals; 


    //Create a new ballot to choose one of the proposal names 

    constructor(bytes32[] memory proposalNames) {
        chairperson = msg.sender; 
        voters[chairperson].weight =1; 


    for (uint i = 0; i < proposalNames.length; i++) {
            // `Proposal({...})` creates a temporary
            // Proposal object and `proposals.push(...)`
            // appends it to the end of `proposals`.
            proposals.push(Proposal({
                name: proposalNames[i],
                voteCount: 0
            }));
        }
    }



    function giveRightToVote(address voter) external {


        require(
            msg.sender == chairperson,
            "Only Chair Person can vote. "
        ); 


        require(
            !voters[voter].voted,
            "The Voter is already voted"
        ); 

        require(voters[voter].weight==0);
        voters[voter].weight =1;


    }



    //delegate the vote to the voter 

    function delegate(address to) external {

        //assigns the reference 


        Voter storage sender = voters[msg.sender];
        require(sender.weight !=0, "You have no right to vote");
        require(!sender.voted, "You are already voted.");
        

        require(to != msg.sender, "Self-delegation is not allowed.");


        //While loop is not good for gas optimisation 


        while (voters[to].delegate != address(0)) {
            to = voters[to].delegate; 


            require(to !=msg.sender, "Found loop in delegation.");
        }

        Voter storage delegate_ = voters[to]; 


        //We also want to make sure voters cannot delegate to account that cant vote 


        require(delegate_.weight >=1);
        sender.voted = true; 
        sender.delegate = to; 
        if(delegate_.voted) {
            //if the delegate already voted,
            //then we can add to the voting 

            proposals[delegate_.vote].voteCount += sender.weight;
        } else {
            //if the delegate did not vote yet 
            //add to her weight


            delegate_.weight += sender.weight; 
        }

    }


    //After all the checks, people can give votes 


    function vote(uint proposal) external {
        Voter storage sender = voters[msg.sender]; 
        require(sender.weight !=0, "Has no right to vote"); 
        require(sender.voted, "Already Voted.");
        sender.voted = true;
        sender.vote = proposal;




        //if the proposal is out of range of array


        proposals[proposal].voteCount += sender.weight; 

    }



    function winningProposals()  public view returns (uint winningProposals_){

                uint winningVoteCount = 0; 
                for (uint p =0; p < proposals.length; p++) {
                    if (proposals[p].voteCount > winningVoteCount) {
                        winningVoteCount = proposals[p].voteCount;
                        winningProposals_ =p;
                    }
                }

}

    function winnerName()external view returns (bytes32 winningName_) {
        winningName_ = proposals[winningProposals()].name;
    }



    
}
