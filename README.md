const {
    MessageEmbed,
} = require('discord.js');

module.exports.run = async (client, message, args) => {
    const member = message.mentions.members.first() || message.guild.members.cache.get(args[0]) || message.member;

    const statuses = {
        online: "Online",
        dnd: "Dnd",
        idle: "Idle",
        offline: "Offline",
    };
    
    let status;
    if (!member.presence) {
        status = 'Unknown;'
    } else {
        status = statuses[member.presence.status]
    }

    const exampleEmbed = new MessageEmbed()
        .setTitle(`${member.user.username}'s Profile`)
        .setColor('#2f3136')
        .setThumbnail(member.user.avatarURL({
            size: 1024,
            dynamic: true,
        }))
        .addFields({
            name: "User Tag",
            value: `${member.user.tag}`,
            inline: true,
        }, {
            name: "ID",
            value: `${member.id}`,
            inline: true,
        }, {
            name: "Status",
            value: `${status}`,
            inline: true,
        }, {
            name: `Roles Count`,
            value: `${message.guild.members.cache.get(member.user.id).roles.cache.size}` || "No Roles!",
            inline: true,
        }, {
            name: `Avatar Url`,
            value: `[Link](${member.user.avatarURL()})`,
            inline: true,
        })
        .setFooter({
            text: `Requested by ${message.author.username}`,
        })
        .setTimestamp();

    message.channel.send({
        embeds: [exampleEmbed],
    });
};

module.exports.config = {
    name: "whois",
    aliases: ['ui'],
};
